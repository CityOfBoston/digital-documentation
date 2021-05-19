# Hosting and monitoring

## Hosting

### Amazon Web Services 

* We use [Terraform](https://www.terraform.io/) for describing our infrastructure. See the [digital-terraform](https://github.com/CityOfBoston/digital-terraform/) repo.
* Do not make one-off changes in the UI or with the command line. Everything should be updated through Terraform so we have transparency about what we’re running and why.
* Terraform changes should be made through [Atlantis](https://www.runatlantis.io/).

### Heroku

* Heroku is appropriate for very one-off apps \(such as the 311 crowdsource app\) where we don’t mind not being on a boston.gov URL or waiting for the dyno to spin up after inactivity.
* We only want to use free tier dynos going forward.
* We need to get off all paid Heroku services, migrating to AWS/S3 as appropriate. Staging apps, however, should generally deploy to AWS to match production-like environments. 
  * [App status / migration plan spreadsheet](https://docs.google.com/a/boston.gov/spreadsheets/d/1rUV91I0PfQK6L4MHGAWdkNc-NJ8Qya3MOT48W70QAwQ/edit?usp=sharing)

## Monitoring

We should never be surprised by an outage. CloudWatch and/or Updown.io should be monitoring our apps and the services they depend on.

If an alarm goes off regularly, either its root cause should be fixed or the alarm should be adjusted or removed.

### CloudWatch

* We have a handful of CloudWatch alarms to monitor our instances, VPN, and services.
* These alarms are sent to the digital-dev@boston.gov email address and posted in the \#digital\_monitoring Slack channel via a custom [Lambda function](https://github.com/CityOfBoston/digital-lambda).

### Rollbar

* We use [Rollbar](https://www.rollbar.com/) to track server-side and client-side exceptions.
* Exceptions we cannot eliminate should be silenced on the Rollbar side or prevented from throwing in the first place. If necessary, they should be converted to logs that can be monitored.

### Updown.io 

* We use [Updown.io](https://updown.io/) for blackbox monitoring of our sites’ availability.
* Yes, parking tickets goes down every night and it’s annoying.

