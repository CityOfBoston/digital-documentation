# New service setup

## Prerequisites

For web services that will run in containers, our system expects the following 2 behaviors:

* It listens for TLS on its `$PORT`
* It responds with a 200 to requests for `/admin/ok`

For static apps that will be served from S3, it’s enough that there’s a script to generate the HTML and JS files.

## Setting up the repository

In general, new services should be written in the [CityOfBoston/digital monorepo](https://github.com/CityOfBoston/digital). This lets us trivially share code, configuration, and conventions that make our apps faster to write and easier to maintain.

We don’t have templates for new services, so your best bet is to copy the most recently written service that is similar to yours. For web applications, choose `registry-certs` or `access-boston`. For non-UI HTTP services, `payment-webhooks`. For a statically-generated site, `public-notices`.

Follow the existing conventions for naming the service’s directory and the `name` property in `package.json`, as the deployment system makes assumptions about where things are located and what they’re named based on this “short name”.

{% hint style="info" %}
It’s best to do a find-and-replace on the old service’s name in the new directory so that you don’t miss anything.
{% endhint %}

## Configuring AWS

You’ll need to update our Terraform configuration for the new service. In the [CityOfBoston/digital-terraform repo](https://github.com/CityOfBoston/digital-terraform), copy the `service_` file that matches the app you copied when setting up the repository and change all the names.

If it’s a while until the public launch, you may want to comment out the production parts of the service.

Besides adding the service, you’ll need to modify `terraform.tvars` to add variants to the `service_variants` variable \(even if it’s just to put `default` in there\) and add lines into `staging_listener_rule_priorities` for each variant. If you’re setting up production now, you’ll need a `prod_listener_rule_priorities` entry as well.

See the [Making changes with Terraform guide](../making-changes-with-terraform.md) for more information about opening a PR and using Atlantis.

## Initial deploys

One you have a skeleton of an app, it’s time to get it running on the ECS clusters.

### Configuration

You’ll likely need to add an appropriate `.env` file to configure your service. See [Service Configuration](../service-configuration/) for more info about our S3 configuration buckets. Since the app is new, you’ll need to create the service subdirectory. Keep the name consistent.

### Staging

The GitHub webhooks and Slack bot will prompt you to deploy if they see changes on a branch that begins with `staging/`. To do the first staging deploy, check in your code locally, and then run:

```bash
$ git checkout -b staging/app-name
$ git push origin -u --no-verify
```

If you’re using variants, use the branch name `staging/app-name@variant`instead.

Shippy-Toe should chime in on \#digital\_builds and offer to deploy to staging. Tell her to do it! She’ll kick off a CodeBuild process to do the deployment.

If you’re staging an S3-based service \(like Public Notices\), you will be able to see it at `https://apps.digital-staging.boston.gov/app-name`.

If your service is container-based, you’ll need to update the ECS service to increase the **Number of tasks** from 0 to 1. Do this via the ECS web console. Once you’re done, your app will be available at `app-name.digital-staging.boston.gov` or `app-name-variant.digital-staging.boston.gov`. \(There’s no additional DNS setup needed.\)

### Production

When you’re ready for production, don’t forget to create configuration files in the prod config bucket. While you hopefully won’t have the same secrets between staging and prod, if you do you’ll need to re-encrypt them because the staging and production services have separate encryption keys.

Shippy-Toe prompts you to do a production deploy \(in the \#digital\_builds channel in slack\) when a Travis run completes on `develop`. Shippy-Toe works out what requires deployment by checking for changes to files in a service’s dependencies that are out-of-sync with that service’s `production/*` branch.

{% hint style="success" %}
**First time deploy for a new app/service.**

The easiest way to do the very first production push for a service is to make a `production/app-name` branch off of `develop` before you make your last set of changes to the app. _**Make sure you push it up to GitHub and merge to `develop`.**_  \(it wont deploy your app yet\).

Make your last PR for the service in a new branch \(off the `develop`branch\) and once it is approved, merge it to `develop.`Once Travis passes, Shippy-Toe will prompt you for a production push \(deploy\) for your app/service.  This will deploy your app to AWS.
{% endhint %}

As with staging, you’ll need to increase the number of tasks from 0. For most web services, you’ll bring it up to 2, one for each availability zone.

If you’re hosting your app at a subdirectory of `apps.boston.gov`, it will be available there. If it’s on its own subdomain, you’ll need to get a DNS change done to point at the ELB.

