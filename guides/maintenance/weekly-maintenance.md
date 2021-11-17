---
description: >-
  Every week some time should be dedicated to reviewing the status and general
  health of Drupal and modules.
---

# Boston.gov Weekly Maintenance

It is not necessary to complete all steps each week. In some weeks the module update will consume 100% of the time available, and other weeks there will be no module updates required, or the decision is made not to update any modules this week.

In general, the maintenance process should start with the review of module updates, as this is a cornerstone of any maintenance program and ensures the balance of issues are being addressed on a completely updated site.

## Activity 1: Use Drupal to check for module updates

There are many ways this task can be effectively completed.  This way has the advantage of being responsive and relatively low-burden between cycles.  

{% hint style="warning" %}
If the basic parts of this process are not conducted weekly, then to ensure the site is kept up-to-date and is adequately patched, there will be a need for monitoring of Drupal/Acquia channels to determine when various patches are available.
{% endhint %}

### Step 1: Check for updates

Login to the production website at [https://content.boston.gov](https://content.boston.gov/) and then navigate to the available updates report page at [/admin/reports/updates](https://content.boston.gov/admin/reports/updates).    
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
</table>

### Step 2: Verify composer version rules.

Following Drupal Best Practices, the downloading and version control of contributed modules \(including Drupal core itself\) is managed by **composer**.  

{% hint style="warning" %}
_**Note:**_  Composer is not fully installed on any Acquia server \(and does not need to be\). Package/Module admin is done by running composer commands locally in development containers.
{% endhint %}

Compare the version rules in your local `composer.json`  with the versions listed on the [status reports page](https://content.boston.gov/admin/reports/updates) in Drupal.  If necessary, change rules in `composer.json` so that the recommended module version will be downloaded.  

{% hint style="info" %}
A useful reference for writing versioning rules in composer can be found [here](https://getcomposer.org/doc/articles/versions.md#writing-version-constraints). 
{% endhint %}

### Step 3: Use Composer to apply updates.

{% hint style="info" %}
**Before you start...**

1. Be sure your git working tree is clean and you have pulled the latest `develop` branch from GitHub.
2. Run `drush cim` on the develop branch to be sure you have the latest yml files imported into your local DB
3. Run `drush cex` to be sure that all configurations are exported  \(if you find configurations are exported then you will need to rebuild your develop environment - or at least sync the local database with the one on Acquia develop - use `lando drush sql:sync @bostond8.dev @self`\).
4. Create a new branch e.g. `maintenance-03-03-2020`
{% endhint %}

Run composer update in your local development container.

```
$ lando composer update
```

Check the output of the composer command, and verify that the modules/packages you expect have been added/remove/updated and that patches have been applied without errors.  If necessary fix the errors by updating the `composer.json` file and re-running **`lando composer update`**.

{% hint style="success" %}
**Tip:** You can see what changes will actually be made when updating by first running:**`lando composer update --dry-run`**   
and comparing the output with the [status reports page](https://content.boston.gov/admin/reports/updates) to see that the required modules have been added, removed and updated.    
_\(**Note**: you can expect composer to update a considerable number of modules which are not listed on the status report because composer also checks dependencies and symfony components that Drupal does not\)_
{% endhint %}

{% hint style="info" %}
You may see patches for a module fail after the version is updated. 

This will either be because the patch is no longer needed \(you can probably work this out by checking the modules release notes\), or because the file is altered and the patch cannot find the anchors it needs to apply the patch.

In the latter case, you may need to modify the patch or check the issue related to the patch and see if there is an updated patch you can apply.
{% endhint %}

{% hint style="danger" %}
If you see this message: 

`Writing lock file Generating autoload files`

`In CopyRequest.php line 91:`

`fopen(/app/docroot/sites/default/default.services.yml): failed to open stream: Permission denied`  
  
Then you need to change the permissions on your local \(i.e. on your host not in the docker container\)`docroot/sites/default`folder to 777, and then rerun the composer command.
{% endhint %}

After the update is completed, update the Drupal caches \(so that the Drupal registries are updated\).

```
$ lando drush cr
```

Then, see if the updated modules need to apply any updates to the database.  
_This step is done to ensure that the updates will apply properly on the Acquia servers when the deploy scripts run this command._

```
$ lando drush updb
```

\[Optional\] We can export the configuration. It is unlikely that there will be changes to configuration, but it is theoretically possible, so to be safe this is a recommended step.  
_Take care with this and ensure that any configuration changes are changes you expect.  You should expect configuration updates only to .yml files from contributed modules updated by composer during this maintenance cycle._

```
$ lando drush cex
```

Finally, commit into the main public d8 repository:  
- the`composer.lock` and \(if changed\) the`composer.json` files -it is very important to include both \(see "final note" box-out below\), and  
- any config files generated from `drush cex` -these will **always** be in the `/config/default` folder.  
_again, take care to check these over, changes to settings files \(in particular\) could affect production settings._

{% hint style="info" %}
**COMPOSER Command Overview:**

**composer update:** \[[notes](https://getcomposer.org/doc/03-cli.md#install-i)\] ****reads the `composer.json` file, works out the package versions which meet all rules \(and recursive dependencies\), compares versions with the existing packages in the local environment and downloads the packages _and dependencies_ which need updating.    
Finally, it updates `composer.lock`with the exact versions of each package that are currently installed.

**composer install:** \[[notes](https://getcomposer.org/doc/03-cli.md#update-u)\] reads the `composer.lock` file, compares to existing packages and downloads those packages _and dependencies_ which need updating.   
_If there is no `composer.lock` file found then composer will read the `composer.json` file and essentially run the `composer update` process._
{% endhint %}

{% hint style="success" %}
**SUMMARY: Recommended overall composer strategy:  
-** Manually maintain the `composer.json` file locally, and use_**`composer update`**_ in the local container to update the `composer.lock` file, then  
- Commit and merge **both** the `composer.lock` and `composer.json` files into the main repository, then  
- Deploy scripts will execute _**`composer install`**_ \(on Travis\) during the site build process so the exact same versions of all packages are deployed on all Acquia servers.
{% endhint %}

{% hint style="info" %}
**Side Note:**  
- The local Lando build script `scripts/local/lando-build-drupal.sh` which is executed by `lando start` , `lando restart` , `lando build`and `lando rebuild`uses **`composer install`** . This is to ensure that the package versions installed locally are the same as the currently loaded package versions on the `dev` Acquia server.  
- Travis script`scripts/deploy/travis_build.sh`\(which builds the full Drupal file system that is deployed to Acquia\) also uses **`composer install.`** This is also to ensure that the package versions loaded into the build/deploy artifact are the exact same package versions as in the local development container environment.
{% endhint %}

{% hint style="info" %}
**Final note \(!\):**

Therefore, the **only way** packages will be updated on the Acquia servers \(ci/uat/dev/stage or prod\) is if a developer runs `composer update` on their local container, and then merges the resultant `composer.lock` file to the `develop` branch of the main repo.    
- If the `composer.lock` file is not merged, then no update will occur.  
- It is not necessary to merge changes to the `composer.json` to complete an update. However, the `composer.json` file still should be committed because if it is not, then it will be out of sync with the current lock file any changes to rules will not be available to another developer.
{% endhint %}

## Step 2: Check Drupal Status Report.

**After** Step 1 has been completed, and all module updates have been applied through to production the [status report](https://content.boston.gov/admin/reports/status) can be checked.  Pay attention to the Errors and Warnings sections of this report.

{% hint style="info" %}
Configuration changes may need to be made to clear warnings and/or errors.

Don't make changes in the UI of an Acquia hosted site - these changes should be made to a development version in a local container, the configs exported and merged into the main repository and then deployed in the normal fashion. 
{% endhint %}

## Step 3: Check Acquia Cloud Console.

{% hint style="info" %}
Acquia provide some useful diagnostic information on their Cloud UI Console.  Much of the information in the Insight reports are taken from Drupal processes behind steps 1-2 above, so checking those areas should be done first.
{% endhint %}

1. Login to the [Acquia Cloud Console](https://cloud.acquia.com/app/develop/all)
2. Navigate to the Drupal 8 Production environment.
3. Click on the "Insight" menu item to view a list of config setting mods recommended by Acquia. **Note:** Acquia Insight provides fairly generic best-practice type comments and it may not be appropriate to adopt/change all of the settings recommended - do some research first, and use common sense.
4. Click on Stack Metrics to view a series of graphs related to resource consumption and basic network performance.  Take a look over these, paying particular attention to disk space and memory utilization graphs.

## Step 4: Site Improve Reports.

{% hint style="info" %}
City of Boston use the external siteimprove.com to monitor the website for slow-pages, missing links etc etc.  Content editors are generally monitoring this site for broken links, but the website contains much information on delivery and performance of boston.gov.
{% endhint %}

login to http://www.mysiteimprove.com and check the console for any significant/new issues highlighted.

