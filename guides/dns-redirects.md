---
description: How we handle redirecting unused domains to their new homes
---

# DNS Redirects

We often get called upon to take old domains, such as data.cityofboston.gov, imagine.boston.gov, and publicartsboston.com, and point them to new homes, typically pages on Boston.gov.

Our DNS provider, DNS Made Easy, does not have a \*.boston.gov HTTPS key, so it can’t handle this on its own.

Making a redirect is a two-step process:

1. Update the “Default Handler” Nginx config to process the host and intended target
2. Change the DNS records at DNS Made Easy to point to our apps cluster’s load balancer

### Nginx Server

The Digital apps cluster has containers running [Nginx](https://www.nginx.com/), a web server, that are configured to be the default handlers for traffic to the production, public, and staging load balancers. Any traffic that is not directed to other containers by listener rules ends up at the Nginx servers.

They serve several purposes:

* Redirecting HTTP traffic to HTTPS \(with the same host name and path\)
* Proxying apps.boston.gov traffic to the static S3 buckets for statically-deployed apps \(like commissions-search and public-notices\)
* Configuring custom redirects

Though there are separate ECS services for each load balancer, they run the same Nginx container images.

The Nginx containers are defined in the private [CityOfBoston/devops](https://github.com/CityOfBoston/devops) repo, in the docker/nginx-http-redirect directory. Instructions on building, testing, and deploying the containers are in its [README](https://github.com/CityOfBoston/devops/blob/master/docker/nginx-http-redirect/README.md).

### DNS changes

All DNS changes have to be done through a change management request, though given their low risk they typically can be turned around same-day, rather than waiting for a change management meeting approval.

Frank Skelton is usually responsible for making the changes in the DNS Made Easy UI, so contact him as you put in the change management request.

For subdomains, \(_e.g._ budget.boston.gov\) you can simply make a CNAME record from the subdomain to the public ALB’s domain name, which is AppsPublicAlb-861712829.us-east-1.elb.amazonaws.com.

For bare domains, like publicartsboston.com which cannot be CNAMEd, you make an “ANAME” record. This is a DNS Made Easy feature that periodically pings the target domain name for its IP addresses, and updates the A record accordingly.

