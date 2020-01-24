---
description: >-
  Every week some time should be dedicated to reviewing the status and general
  health of Drupal and modules.
---

# Weekly Maintenance

There are many ways this task can be effectively completed.  This way has the advantage of being responsive and relatively low-burden between cycles.  

{% hint style="warning" %}
If the basic parts of this process are not conducted weekly, then to ensure the site is kept up-to-date and is adequately patched, there will be a need for monitoring of Drupal/Acquia channels to determine when various patches are available.
{% endhint %}

## Activity 1: Use Drupal to check for module updates

### Step 1: Check for updates

Login to the production website at [https://content.boston.gov](https://content.boston.gov/) and then navigate to the status report page at [/admin/reports/updates](https://content.boston.gov/admin/reports/updates).    
All available updates which managed by composer are listed on this page.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Update Type</th>
      <th style="text-align:left">Action to take</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Security Updates</b>
      </td>
      <td style="text-align:left">These should be applied ASAP.
        <br /><em>Ideally these are deployed as the only change in a new commit.</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Normal Updates</b>
      </td>
      <td style="text-align:left">
        <p>It is not necessary to rush these updates and time can be taken to investigate
          what will be affected and consider if this will affect the website.</p>
        <p>Even though these are not important, it is best practice to apply updates
          as they are ready because not applying may block other updates from happening.
          <br
          /><em>It may be convenient to include in the next scheduled deploy.</em>
        </p>
      </td>
    </tr>
  </tbody>
</table>### Step 2: Verify composer version rules.

Following Drupal Best Practices, the downloading and version control of contributed modules \(including Drupal core itself\) is managed by **composer**.  

{% hint style="warning" %}
_**Note:**_  Composer is not fully installed on any Acquia server \(and does not need to be\). Package/Module admin is done by running composer commands locally in development containers.
{% endhint %}

Compare the version rules in your local `composer.json`  with the versions listed on the [status reports page](https://content.boston.gov/admin/reports/updates) in Drupal.  If necessary, change rules in `composer.json` so that the recommended module version will be downloaded.  

{% hint style="info" %}
A useful reference for writing versioning rules in composer can be found [here](https://getcomposer.org/doc/articles/versions.md#writing-version-constraints). 
{% endhint %}

### Step 3: Use Composer to apply updates.

Run composer update in your local development container.

```
$ lando composer update
```

Check the output of the composer command, and verify that the modules/packages you expect have been added/remove/updated and that patches have been applied without errors.  If necessary fix the errors by updating the `composer.json` file and re-running **`composer update`**.

{% hint style="info" %}
You may see patches for a module fail after the version is updated. 

This will either be because the patch is no longer needed \(you can probably work this out by checking the modules release notes\), or because the file is altered and the patch cannot find the anchors it needs to apply the patch.

In the latter case, you may need to modify the patch or check the issue related to the patch and see if there is an updated patch you can apply.
{% endhint %}

After the update is completed, update the Drupal caches \(so that the Drupal registries are updated\).

```
$ lando drush cr
```

Finally we should export the configuration. It is unlikely that there will be changes to configuration, but it is theoretically possible, so to be safe this is a recommended step.

```
$ lando drush cex
```

Finally commit the changed `composer.json` and `composer.lock` \(it is very important to include both - see composer box-out below\) and any config files \(which will **always** be in the `/config/default` folder\) into the main public repository.

{% hint style="info" %}
**composer update:** \[[notes](https://getcomposer.org/doc/03-cli.md#install-i)\] ****reads the `composer.json` file, works out the package versions which meet all rules \(and recursive dependencies\), compares versions with the existing packages in the local environment and downloads the packages which need updating.    
Finally, it updates `composer.lock`with the exact versions of each package that are currently installed.

**composer install:** \[[notes](https://getcomposer.org/doc/03-cli.md#update-u)\] reads the `composer.lock` file, compares to existing packages and downloads those packages which need updating.   
_If there is no `composer.lock` file found then composer will read the `composer.json` file and essentially run the `composer update` process._
{% endhint %}

{% hint style="success" %}
**SUMMARY: Recommended overall composer strategy:  
-** Manually maintain the `composer.json` file locally, and use_**`composer update`**_ in the local container to update the `composer.lock` file, then  
- Commit and merge the `composer.lock` and `composer.json` files into the main repository, then  
- Deploy scripts will execute _**`composer install`**_ \(on Travis\) during the site build process so the exact same versions of all packages are deployed on all Acquia servers.
{% endhint %}

## Step 2: Check Drupal Status Report.

**After** Step 1 has been completed, and all module updates have been applied through to production the [status report](https://content.boston.gov/admin/reports/status) can be checked.  Pay attention to the Errors and Warnings sections of this report.

{% hint style="info" %}
Configuration changes may need to be made to clear warnings and/or errors.

Don't make changes in the UI of an Acquia hosted site - these changes should be made to a development version in a local container, the configs exported and merged into the main repository and then deployed in the normal fashion. 
{% endhint %}

## Step 3: Check Acquia Cloud Console.

1. Login to the [Acquia Cloud Console](https://cloud.acquia.com/app/develop/all)
2. Navigate to the Drupal 8 Production environment.

## Step 4: Site Improve Reports.

