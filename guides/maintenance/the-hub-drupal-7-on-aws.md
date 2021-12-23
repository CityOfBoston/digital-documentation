# The Hub (Drupal 7 on AWS)

Weekly maintenance for the Hub is slightly different because it is a legacy Drupal 7 site.

Login to [https://hub.boston.gov](https://hub.boston.gov) as an administrator and then open the [Update Status](https://hub.boston.gov/admin/reports/updates/update) page in the Hub UI.  Check the list of available updates - look specifically for **security updates** (usually highlighted in red).\
_**If there are updates you want to perform then:**_\
_****_Switch to your _**local build**_  (https://hub.lndo.site), and ensure you have the latest `develop` branch checked out and that it is up to date with the remote (AWS CodeCommit) repo.  Verify that your local version is requiring the same updates are required on the [local Update Status ](https://hub.lndo.site/admin/reports/updates/update)page (if not then check the branch and possibly sync the local DB with a version from Prod or Dev).

{% hint style="warning" %}
If any updates or changes need to be made, then backup your local database before starting, it may save time and heartache later !!!
{% endhint %}

### **If Drupal itself needs updating then:**

1. Ensure your git working tree is clean (i.e no uncommitted changes to files)
2. Download the latest Drupal core release archive from [drupal.org](https://www.drupal.org/project/drupal/releases?version=7).
3. Unzip the archive into a local folder
4. Open a terminal and go to the folder you just unzipped.
5. Run `rsync -arz -essh -P . /[abs-path]/docroot/`(where abs-path is the folder where you have the repo checked out)
6. Check that files have copied correctly
7.  Check the changes being reported by git, especially "key file"s to see if you want them changed (usually you don't).  \
    This is a list of "key files":

    * .htaccess files (especially the one in the docroot)
    * any settings files you find in /docroot/sites/default

    revert or rollback any changes you do not wish to keep (e.g. `.htaccess` file)&#x20;
8. Run `lando drush cc all` and `lando drush updb`
9. Check the site at `https://hub.lndo.site`and see that it is working properly.
10. Commit the changes and deploy them.
11. Delete the local folder from step 2 above.

### **For all other **_**non-Drupal core**_** module updates**

* On your local version of the website (https://hub.lndo.site)
* Make sure you have the latest version of the `develop` branch checked out and up to date with the remote (AWS CodeCommit) repo.
* Open the [Update Status](https://hub.lndo.site/admin/reports/updates/update) page in the UI.\
  &#x20;\- or -\
  `lando drush ups`
* Note all the updates you wish to apply (from page admin/reports/updates/update).
* Switch to the docroot folder on your host computer and run `lando drush up modulename1, modulename2 ...`\
  alternatively you can do this to update all outdated contributed modules in one go:\
  `lando drush up --no-core --notes [--security-only] [--entity-updates]`
* The updates will be downloaded, and then you will be given a list of updates that will be performed, check this with the list noted above. If all is OK type `yes` at the prompt to apply the updates. (Note: database updates are applied by drush).
* [Check the updates](https://hub.lndo.site/admin/reports/updates/update) have been applied.
* Commit the changes and deploy them.

### Other weekly checks

#### HCM Feed (Active Directory Sync)

* Check [https://hub.boston.gov/admin/content/migrate/groups/hub-user](https://hub.boston.gov/admin/content/migrate/groups/hub-user) to see that the HCM import is not stuck `importing` or some non-idle state.  If it is, then reset each of the **User** and **Profile** migration tasks so they show `idle` as their status.\
  You can ssh to the eb server (`$ eb ssh HubProduction`) and then check the contents of the log file `$ cat /var/log/drupal/drush-hcm.log`, check the script at `$ sudo vim /var/app/current/docroot/sites/default/files-private/drush_HCM.sh`and finally check cron launches the script properly `sudo crontab -l` you can also manually run the script using `$ . /var/app/current/docroot/sites/default/files-private/drush_HCM.sh`
