---
description: >-
  When and if a new environment is setup on Acquia for CoB, the following steps
  should be followed:
---

# Acquia Environment setup checklist

{% hint style="info" %}
When a new environment is added, it will have a 3-4 character name \(e.g. `uat` or `dev2` etc\).  This checklist refers to this environment short-name as the **envname**.
{% endhint %}

### Acquia Hooks

* [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) edit the Acquia Hook scripts in the `/hooks/common` folders, looking for the following code in the`post-code-update.sh` and `post-code-deploy.sh`files:

  ```text
  if [[ "${target_env}" == 'uat' ]] || [[ "${target_env}" == 'ci' ]] ... ; then
  ```

  and adding in a new condition for the new envname:

  ```text
  ... || [[ "${target_env}" == 'envname' ]] ...
  ```

  save the updated files and commit to the private repo.

* [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) find the following line in the script at`/hooks/common/cob_utilities.sh` file \(approx line 270\):

  ```text
  elif [[ "${target_env}" == "ci" ]] || [[ "${target_env}" == "uat" ]] || ... ; then
      site_machine_name="none"
  fi
  ```

  add in the a new condition for the new envname:

  ```text
  ... || [[ "${target_env}" == "envname" ]] ...
  ```

  save the updated file and commit to the private repo.

### Drush

* [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) edit the drush environment definition script in the `/drush/sites/bostond8.site.yml` file.  Add a new entry at the bottom of the file:

  ```text
  envname:
    root: /var/www/html/bostond8.envname/docroot
    ac-site: bostond8
    ac-env: envname
    ac-realm: prod
    uri: d8-envname.boston.gov
    host: bostond8envname.ssh.prod.acquia-sites.com
    user: bostond8.envname
    envname.livedev:
      parent: "@bostond8.envname"
      root: "/mnt/gfs/bostond8.envname/livedev/docroot"
    paths:
      drush-script: drush9
  ```

  \(replace envname with the new environment name\).  
  Save the updated file and commit to the private repo.

### Acquia Cloud

* [ ] Login to the [Acquia Cloud Console](https://cloud.acquia.com/app/develop/all) and click on the bostond8 application.
* [ ] Click on the new environment to open the configuration page for that environment.
* [ ] Click on "Scheduled Jobs" and create the following new Job: **Job name:** Envname Site Cron **Command:** `cd /var/www/html/${AH_SITE_NAME}/docroot && drush @bostond8.envname cron -dv &>> /var/log/sites/${AH_SITE_NAME}/logs/$(hostname -s)/drush-cron.log` **Command frequency:** `*/15 * * * * (entered as a string)`
* [ ] Click on "Variables" and create variables copied from the `dev` environment.
* [ ] \[optional\] Click on Domains and add an edit sub-domain. \(this is requested from the LAN team, and should be in the pattern of `d8-envname.boston.gov`. Ask them to set it up the same way that d8-`dev.boston.gov` is set up.  The DNS they create would ideally be private \(only to city hall\) but they may need to make it on the public DNS servers, which is OK too\).  

### Using the Environment

See [On Demand](./) section.

