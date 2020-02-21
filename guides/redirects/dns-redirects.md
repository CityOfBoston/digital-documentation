---
description: How we handle redirecting unused domains to their new homes
---

# DNS Redirects

We often get called upon to take old domains, such as data.cityofboston.gov, imagine.boston.gov, and publicartsboston.com, and point them to new homes, typically pages on Boston.gov.

Our DNS provider, DNS Made Easy, does not have a \*.boston.gov HTTPS key, so it can’t handle this on its own.

Making a redirect is a two-step process:

1. Update the ALB config to process the host and intended target
2. Change the DNS records at DNS Made Easy to point to our apps cluster’s load balancer

### ALB config

Our public load balancer is configured with rules for static redirects. This configuration is kept in Terraform.

To add a new redirect, edit the [terraform.tfvars](https://github.com/CityOfBoston/digital-terraform/blob/production/apps/terraform.tfvars) file. The `prod_redirects` variable holds the configuration for these redirects.

Once that’s done, make a PR and use Atlantis to apply the Terraform changes. See the Making changes with Terraform guide.

### DNS changes

All DNS changes have to be done through a change management request, though given their low risk they typically can be turned around same-day, rather than waiting for a change management meeting approval.

Frank Skelton is usually responsible for making the changes in the DNS Made Easy UI, so contact him as you put in the change management request.

For subdomains, \(_e.g._ budget.boston.gov\) you can simply make a CNAME record from the subdomain to the public ALB’s domain name, which is AppsPublicAlb-861712829.us-east-1.elb.amazonaws.com.

For bare domains, like publicartsboston.com which cannot be CNAMEd, you make an “ANAME” record. This is a DNS Made Easy feature that periodically pings the target domain name for its IP addresses, and updates the A record accordingly.

