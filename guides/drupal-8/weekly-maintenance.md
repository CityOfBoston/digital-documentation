---
description: >-
  Every week some time should be dedicated to reviewing the status and general
  health of Drupal and modules.
---

# Weekly Maintenance

There are many ways this task can be effectively completed.  This way has the advantage of being responsive and relatively low-burden between cycles.  

{% hint style="info" %}
If the basic parts of this process are not conducted weekly, then to ensure the site is kept up-to-date and is adequately patched, there will be a need for monitoring of Drupal/Acquia channels to determine when various patches are available.
{% endhint %}

## Step 1: Use Drupal to check for module updates

Login to the production website at [https://content.boston.gov](https://content.boston.gov/) and then navigate to the status report page at [/admin/reports/updates](https://content.boston.gov/admin/reports/updates).    
All available updates managed by composer are listed on this page, and security updates are shaded red \(and can be filtered separately\).

**Security Updates**: Should be applied ASAP, and tested on production.

**Updates:** Can be applied after some investigation as to what will be affected.  It is not necessary to rush these updates into production, and it may be convenient or appropriate to make any necessary changes to `composer.json` and then include in the next scheduled deploy.

### Update composer.

Following Drupal Best Practices, the downloading and version control of contributed modules \(including Drupal core itself\) is managed by **composer**.  Note: Composer is not installed on any Acquia server \(and should not be\), management is done by running composer commands on .

Compare the version rules in your local `composer.json`  with the versions listed on the [status reports page](https://content.boston.gov/admin/reports/updates) in Drupal  If necessary, alter the `composer.json` file so the rules will cause the recommended module version to be downloaded. A useful reference for versioning rules in composer can be found [here](https://getcomposer.org/doc/articles/versions.md#writing-version-constraints). 

## Step 2: Use Composer to apply updates.

The easiest way is to run composer update in your container, update the Drupal caches and then export the changed config \(there most likely wont be any changes.\).

```
$ lando composer update
$ lando drush cr
$ lando drush cex
```

Then commit the changed `composer.json` and `composer.lock` \(Very important to include both - see composer box-out below\) into the main repo.

{% hint style="info" %}
**composer update:** reads the `composer.json` file, works out the package versions which meet all rules \(and recursive dependencies\), compares to existing packages, downloads the packages which need updating and finally updates `composer.lock`.

**composer install:** reads the `composer.lock` file, compares to existing packages and downloads those packages which need updating.
{% endhint %}

{% hint style="success" %}
**Current Strategy.  
-** Manually maintain the `composer.json` file locally, and use _**`composer update`**_ locally to generate the `composer.lock` file.    
- Run _**`composer install`**_ only in deployment scripts so the exact same versions of all packages are deployed on Acquia servers until the `composer.lock`file is changed, verified on a local build and then checked into the github repository.
{% endhint %}

## Step 3: Check logs for errors.

