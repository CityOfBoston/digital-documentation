---
description: >-
  Descriptions of GitHub accounts we’ve need to make that don’t belong to
  people.
---

# GitHub Service Accounts

These accounts should all be members of the (secret) [Digital Service Accounts](https://github.com/orgs/CityOfBoston/teams/digital-service-accounts) GitHub team so we can keep track of them.

**cob-deployer** (Twicki) — This is a legacy service account that appears not to be currently used. It has 2-factor auth on but no registered SSH keys, so it doesn’t seem like any automated process can be using it. (The cloning of boston.settings as part of the Travis build comes from a “deploy key” specifically added to that repo.) As of 5/22 this account has no special permissions, and probably can be deleted if we can go a few weeks of deploys with nothing breaking.

**cob-digital-atlantis** (Atlantis) — Account for the Atlantis app used in Terraform deployment. Needs permission to the private digital-terraform repo so that it can see the Terraform templates and post its comments about plan and apply success and failure. (See: [CityOfBoston/digital-terraform](https://github.com/CityOfBoston/digital-terraform))

**cob-digital-bot** (Shippy-Toe) — Used by the internal-slack-bot tool as part of Digital monorepo deployments. Needs write access to digital repo so it can update `production` branches as part of deployment. (See: Digital Webapp DeploymenT)

**cob-heroku **(City of Boston Heroku Deployer) — Used for connecting GitHub and our dwindling number of Heroku apps. At this point just needs read/write access to patterns so it can make apps for each PR and report the deployment back. (Admin access is needed only when first connecting the account to Heroku so it can set up webhooks.)
