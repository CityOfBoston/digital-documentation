# The Hub \(Drupal 7 on AWS\)

Weekly maintenance for the Hub is slightly different because it is a legacy Drupal 7 site.

Login to [https://hub.boston.gov](https://hub.boston.gov) as an administrator and then open the [Update Status](https://hub.boston.gov/admin/reports/updates/update) page in the Hub UI.  Check the list of available updates - look specifically for **security updates** \(usually highlighted in red\).  
_**If there are updates you want to perform then:**_  
Switch to your _**local build**_  \(https://hub.lndo.site\), and ensure you have the latest `develop` branch checked out and that it is up to date with the remote \(AWS CodeCommit\) repo.  Verify that your local version is requiring the same updates are required on the [local Update Status ](https://hub.lndo.site/admin/reports/updates/update)page \(if not then check the branch and possibly sync the local DB with a version from Prod or Dev\).

{% hint style="warning" %}
If any updates or changes need to be made, then backup your local database before starting, it may save time and heartache later !!!
{% endhint %}

### **If Drupal itself needs updating then:**

* Check that the rule in `composer.json` will allow the update. e.g. to update from v7.58 to v7.61 the rule must allow this - reccomend that the rule used for Drupal is `drupal/drupal: 7.*`.
* Open a terminal on your local machine and change to the repository root \(parent folder for the `docroot` folder- i.e. the root for drupal\)
* execute `lando composer update` to cause the required modules in `composer.json` file to  be checked for more recent versions, and based on the version rules then downloads the latest eligible version for each.
* Run `lando drush cc all` and `lando drush updb` to execute any database changes etc.
* Commit the updated files to the repo, and deploy.

{% hint style="info" %}
If you are having issues using composer to do the update \(e.g. composer reports that there is nothing to update ... \) then follow these steps:

1. Download the latest drupal core release archive from [drupal.org](https://www.drupal.org/project/drupal/releases?version=7).
2. Unzip the archive into a local folder
3. Open a terminal and goto the folder you just unzipped.
4. Run `rsync -arz -essh -P . /[abs-path]/docroot/`
5. Check that files have copied correctly
6. Run `lando drush cc all` and `lando drush updb`
7. Commit changes and deploy.
{% endhint %}

### **For all other** _**non-Drupal core**_ **module updates**

* On your local version of the website \(https://hub.lndo.site\)
* Make sure you have the latest version of the `develop` branch checked out and up to date with the remote \(AWS CodeCommit\) repo.
* Open the [Update Status](https://hub.lndo.site/admin/reports/updates/update) page in the UI.
* Select the all updates you wish to apply, and then click the "Download these Updates" button
* The updates will be downloaded, and then you will be prompted to apply the updates, **Do not put the site in maintenance mode** \(because you are running locally, this is not required\).
* After the download is complete, you will see a list of all updates applied, and at the bottom of the page a link to "Run Database Updates". Even though you are working locally and therefore DB changes applied will remain local - you should click this link to allow that to happen to be sure there are no errors.
* Commit the changes and deploy.

