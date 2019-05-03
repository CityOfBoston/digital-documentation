# Deployment

## Continuous deployment apps

{% hint style="info" %}
For our S3 and Node-based apps, which can be pushed to production any time.
{% endhint %}

* We push changes to production as soon as they’re ready. This keeps us from piling up inventory. Shipping small batch sizes is also safer.
* We’ve built a deployment system that has zero-downtime, so it’s always safe to push.
* See each repo’s documentation for instructions on how to deploy it.

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

