---
description: 'Info about https://permitfinder.boston.gov/'
---

# Permit Finder

Permit Finder is a tool for looking up the status of a building or fire permit. It is friendlier to use than Hansen and allows non-authenticated access and shows the name of the inspector currently assigned to review the permit.

The implementation in the Digital monorepo is a rewrite of the original Permit Finder, which was written in PHP/jQuery by Qlarion.

Permit Finder currently receives about 500 unique users a week.

### Documentation

* [PermitFinder Product Proposal](https://docs.google.com/document/d/1BKtVSitjEiEv1vmrlUCppdAxxwYZRNoNIbLO75VNnmY/edit)
* [Permit Finder ETL Documentation](https://docs.google.com/document/d/1PlwKWRNHg688VSRPOYYTtBkFwWGqT-2VS1S5jYWRoI0/edit)
* Code archive: [PMv3.27.zip](https://drive.google.com/file/d/17j0C7EJrqHkRvKjSubFs4adzigAN7cJP/view?usp=sharing)

### Data Flow

Permit Finder data is pulled out of Hansen by an ETL job running in Civis. This runs every 30 minutes. It generates 3 CSV files:

* DataElementExport.csv
* MilestoneExport.csv
* ReviewExport.csv

These files contain information about the permit, the most recent milestone it has hit, and any reviews for the permit, respectively. All of the data is keyed off of the permit number. All dates and times are in Eastern time.

The ETL job exports the data via SFTP.

* In the previous implementation, this was by SFTPing to the EC2 instance that was running the webserver. The PHP scripts would load and parse the CSV for each request.
* In the rewrite, the SFTP destination is an [AWS Transfer](https://aws.amazon.com/sftp/) endpoint in the Digital team’s AWS account. AWS Transfer uses S3 as its backend. In our case, Civis authenticates as the `civis` user using an SSH private key and the data gets written to the `cob-digital-analytics-buckets` S3 bucket under `permit-finder/`.

The AWS Transfer endpoint, `civis` user, and bucket are all generated from Digital’s Terraform templates: [sftp.tf](https://github.com/CityOfBoston/digital-terraform/blob/production/apps/sftp.tf) and [analytics\_uploads.tf](https://github.com/CityOfBoston/digital-terraform/blob/production/apps/analytics_uploads.tf). If the user’s public key needs to be changed, or if new users or buckets need to be added, that can be done from those templates.

The containerized Node servers in the new version periodically query S3 to see if the files’ last modified date has changed. If it has, they stream the files in, parsing the CSV, and write the rows to a local, temporary [Level](https://github.com/google/leveldb) database. \(See: [PermitFiles.ts](https://github.com/CityOfBoston/digital/blob/develop/services-js/permit-finder/src/server/services/PermitFiles.ts)\)

{% hint style="info" %}
**What’s Level?** Level, developed by Google and used in Chrome, is a very fast disk-based key-value store with a straightforward Node API.

There’s too much data to store comfortably in memory in the Node processes, but we don’t need the added complexity of a completely separate database or Redis store.

Level is a happy medium between the two that keeps the data localized in the Node process’s container \(the databases are created under `/tmp`\) but without taking up too much memory.
{% endhint %}

### Next Steps

Currently the rewrite is available on staging at [permit-finder.digital-staging.boston.gov](https://permit-finder.digital-staging.boston.gov). Its UI is feature-complete relative to the current production implementation.

We have successfully tested out having Civis upload the files through the AWS Transfer endpoint and loaded them in the app.

To go to production, we need:

* Product / QA signoff
* The ETL job to get scheduled to run regularly
* Setting the number of tasks in the Digital Prod cluster from 0 to 2
* DNS change for permitfinder.boston.gov

### Risks

This code was written very fast by someone in her last days at the City. So there’s that. She apologizes in advance.

We’re re-using the existing SFTP scheme for the ETL job, so that part should stay relatively stable, though we’re now SFTPing to an AWS Transfer instance rather than directly to a Linux server. We’ve validated login and upload, though, so we expect this to keep working, and potentially be even more reliable since AWS Transfer is a managed service that Amazon is responsible for maintaining.

Level itself is well-proven technology, but this is the first time we’re using it in one of our webapps. There is a chance that its disk usage, or the Docker volume it’s on, will grow over time, despite us deleting stale permit data. Unfortunately, the AWS web console does not provide any disk usage information about EBS block device, so you have to monitor disk usage by SSHing into the box and running `df -hT /dev/xvda1` to see how much disk is being used overall. The Permit Finder container’s `/tmp` directory will be somewhere under `/var/lib/docker/volumes`, though you’ll need to `sudo -s` from `ec2-user` to `root` to see it.

### Future Directions

The rewrite maintained the SFTPing of CSV files just to keep the number of changes small, especially on the Analytics side. This was done to reduce risk.

Here are areas for improvement:

* Have Civis upload to S3 directly. We could enable the Civis account to have access to the Digital team’s S3 bucket via IAM permissions. Civis could put the CSV files there directly, allowing us to remove the AWS Transfer endpoint \(unless some other process has adopted it in the meantime\).
* Use a format other than CSV files. It would be ideal if Civis could generate a file per permit in JSON format, with the milestone and review data collected in it. Those files could be put directly in S3, and the web server could just download the specific permit’s data from S3 and not need to keep its own local store.  

