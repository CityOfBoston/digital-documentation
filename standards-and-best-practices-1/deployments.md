# Deployment

## Continuous deployment apps

{% hint style="info" %}
For our S3 and Node-based apps, which can be pushed to production any time.
{% endhint %}

* The repo’s default branch is `production`, and PRs should be merged to it and deployed presently. It must be made “protected” in GitHub.
* Repos have a `staging` branch that can be force-pushed to for deployment to a staging environment, for changes that need production-like validation before being available publicly.

## Fixed-schedule deployments

{% hint style="info" %}
Boston.gov and the Hub are deployed on a fixed schedule rather than continuously, owing to the disruption of pushing new Drupal changes.
{% endhint %}

* Changes are made against a `develop` branch rather than `production`
* `develop` branch is pushed to a staging environment on a weekly schedule
* Issues in “Inventory” Kanban column get moved to “Staging” column
* Issue creator / feature owner validates fixes on staging
* Code gets merged to `production` branch and pushed to production the following week
* Issues move from “Staging” column to “Closed” after pushing to Production

_Possible change:_ Adding a “Staging” lane to differentiate with “Inventory” \(not yet pushed to staging\).

