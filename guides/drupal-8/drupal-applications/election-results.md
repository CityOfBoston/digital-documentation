---
description: '-- OUT OF DATE Feb 2023 --'
---

# Election results

Digital team is responsible for showing unofficial election results on Boston.gov.

On election night, an HTML file of partial results is generated during tabulation and copied to the cityofboston.gov’s web server. The contents of this file are inserted into the Boston.gov unofficial election results page via a client-side AJAX request.

Historically, election night traffic for these results has been more than cityofboston.gov was comfortably able to handle. We previously ran a job on Heroku that copied the page from cityofboston.gov and put it on an S3 bucket, and the Boston.gov page referenced that S3 version.

As of January 2019, we’ve modified both the source file and our Incapsula settings so that Incapsula caches all requests for the file for 60s. This let us turn off the Heroku job.

#### Links

* **Production:** [https://www.boston.gov/departments/elections/unofficial-election-results](https://www.boston.gov/departments/elections/unofficial-election-results)
* **Source:** [https://www.cityofboston.gov/elections/CurrentElections/fetch.aspx](https://www.cityofboston.gov/elections/CurrentElections/fetch.aspx)
