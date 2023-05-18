---
description: Introduction to how the Digital webapps infrastructure is set up in AWS
---

# Production Overview

![Diagram of our AWS network structure](../../../.gitbook/assets/digital-aws-diagrams.svg)

{% file src="../../../.gitbook/assets/digital-aws-diagram.xml" %}
Draw.io XML for network structure diagram
{% endfile %}

The Digital team uses [Docker](https://www.docker.com/) via Amazon’s [Elastic Container Service](https://aws.amazon.com/ecs/) to deploy its webapps. We migrated to AWS from Heroku primarily so we could establish a VPN connection to internal City databases (such as those used for boards and commissions applications and the Registry certificate ordering).

Our production cluster runs two copies of each app, one in each of two AZs. This is more for resilience against AZ-specific failures than for sharing load.

Almost all of our AWS infrastructure is described by and modified using our [Terraform configuration](https://github.com/CityOfBoston/digital-terraform).

### Why Docker?

The webapps that the City has developed so far are extremely small and low-traffic. Docker containers let us pack a few machines with as many webapps as we can; right now we’re limited only by memory. Docker keeps these apps isolated from each other. It also makes it easy to do rolling, zero-downtime deployments of new versions.

The typical limitations of Docker (stable storage is a pain, as is running related processes together, loss of efficiency for high loads) are not concerns for the types of apps we’re building.

Amazon’s ECS, along with its Application Load Balancers, handle restarting crashed jobs and routing traffic to the containers.

## Network Configuration

Our app containers are run on EC2 instances that live in four private subnets (2 AZs × 2 environments). These instances do not have public IPs and therefore cannot communicate directly with the public internet, which gives us some level of safety through isolation.

These ECS cluster instances receive traffic from Amazon’s ALB load balancers, which live in corresponding public subnets. They can contact public web services through NAT gateways, which also live in the public subnets. The ECS cluster instances also have access to internal City datacenters through the VPN gateway.

The instances are further isolated by having security groups that only allow traffic from the security groups of their corresponding ALBs (and SSH traffic from the bastion instance).

### VPN Gateway

The VPN gateway connects from our VPC to the City datacenter. It has two connections running simultaneously for redundancy. AWS VPNs need to have regular traffic to keep them active, and if they do disconnect they need traffic from outside AWS to cause them to come back online.

We have a SiteScope rule set up with the CoB network team that pings an EC2 instance inside of our VPC. (Currently this EC2 instance does not seem to be created via Terraform.) This rule does a ping every few minutes, which keeps traffic running on the connection and also will bring it back up if it does go down.

Additionally, we have a CloudWatch alarm that fires if one or both of the VPN connections goes down. If one has gone down traffic should still be flowing over the other, and usually it will come back up of its own accord. Contact NOC if there are issues.

### SSH Access

In general, you should not need to SSH on to the cluster instances. Definitely not for routine maintenance (do that through an ECS task if you need that kind of thing). It may be necessary to troubleshoot and debug issues, however.

Instructions for how to SSH on to our bastion machine using an SSH key loaded into your IAM account, and from there how to SSH on to a cluster instance, are in the [digital-terraform’s README.md](https://github.com/CityOfBoston/digital-terraform#bastion-access) file.
