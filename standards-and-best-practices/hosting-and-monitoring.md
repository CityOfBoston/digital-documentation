# Hosting & monitoring

## Hosting

### Amazon Web Services 

* All AWS setup should be done through CloudFormation templates. No one-off changes allowed.
* We use [Terraform](https://www.terraform.io/) for new services and deploys.
* Generic CloudFormation templates are checked in to the [devops](https://github.com/CityOfBoston/devops) repo. Services will also typically have a setup.yml template for service-specific customizations.
* We should never be surprised by an outage. CloudWatch should be monitoring our apps and the services they depend on.
* If an alarm goes off regularly, either its root cause should be fixed or the alarm should be adjusted or removed.

### Heroku

* Heroku is appropriate for very one-off apps \(such as the 311 crowdsource app\) where we don’t mind not being on a boston.gov URL or waiting for the dyno to spin up after inactivity.
* We only want to use free tier dynos going forward.

## Monitoring

### Rollbar

* We use [Rollbar](https://www.rollbar.com/) to track server-side and client-side exceptions.
* Exceptions we cannot eliminate should be silenced on the Rollbar side or prevented from throwing in the first place. If necessary, they should be converted to logs that can be monitored.

### Updown.io 

* We use [Updown.io](https://updown.io/) for blackbox monitoring of our sites’ availability.
* Yes, parking tickets goes down every night and it’s annoying.

