---
description: >-
  When and if a new environment is setup on Acquia for CoB, the following steps
  should be followed:
---

# Acquia Environment setup checklist

{% hint style="info" %}
When a new environment is added, it will have a 3-4 character name (e.g. `uat` or `dev2` etc).  This checklist refers to this environment short-name as the **envname**.
{% endhint %}

### Acquia Hooks

*   [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) edit the Acquia Hook scripts in the `/hooks/common` folders, looking for the following code in the`post-code-update.sh` and `post-code-deploy.sh`files:

    ```
    if [[ "${target_env}" == 'uat' ]] || [[ "${target_env}" == 'ci' ]] ... ; then
    ```

    and adding in a new condition for the new envname:

    ```
    ... || [[ "${target_env}" == 'envname' ]] ...
    ```

    save the updated files and commit to the private repo.\

*   [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) find the following line in the script at`/hooks/common/cob_utilities.sh` file (approx line 270):

    ```
    elif [[ "${target_env}" == "ci" ]] || [[ "${target_env}" == "uat" ]] || ... ; then
        site_machine_name="none"
    fi
    ```

    add in the a new condition for the new envname:

    ```
    ... || [[ "${target_env}" == "envname" ]] ...
    ```

    save the updated file and commit to the private repo.

### Acquia Settings

*   [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) find the following line in the script at`/sites/default/settings/hooks/common/cob_utilities.sh` file (approx line 63):

    ```
    ...
    elseif ($_ENV['AH_SITE_ENVIRONMENT'] == 'uat') {
      $conf['acquia_purge_domains'] = array(
        'bostond8uat.prod.acquia-sites.com',
      );
    }
    ...
    ```

    add in a new else statement for the new environment:

    ```
    elseif ($_ENV['AH_SITE_ENVIRONMENT'] == 'envname') {
      $conf['acquia_purge_domains'] = array(
        'bostond8envname.prod.acquia-sites.com',
        'd8-envname.boston.gov',
      );
    }
    ```

    (replace "envname" with the new environment name)

{% hint style="info" %}
This change adds the specified domains to the acquia-purge registry.  This means the varnish cache for these domains will be automatically purged.  If a sub-domain is attached to an environment and is NOT listed here, then it will not be automatically purged as content is changed.
{% endhint %}

*   [ ] In the same file find the following section (around line 79):

    ```
    ...
    if (in_array($siteEnv, ["test", "ci", "uat"])) {
      $settings['file_public_path'] = 'sites/default/files/linked';
    ...
    ```

    and add the new environment name to the list of environments.

    ```
    if (in_array($siteEnv, ["test", "ci", "uat", "envname"])) {
    ```

    save the updated file and commit to the private gitHub repository.

{% hint style="info" %}
This change directs the new environment to request images and files from a shared (linked) folder rather than the default `sites/default/files` folder.  The folder is linked to conserve file space as each environment basic requires the same sets of images and files.
{% endhint %}

### Drush

*   [ ] In the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) edit the drush environment definition script in the `/drush/sites/bostond8.site.yml` file.  Add a new entry at the bottom of the file:

    ```
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

    (replace envname with the new environment name).\
    Save the updated file and commit to the private gitHub repository.

### .htaccess

*   [ ] In the `.htaccess` file in the CoB Drupal 8 [private gitHub repository](https://github.com/CityOfBoston/boston.gov-d8-private) alter the domain filters as follows:\
    **Everywhere** you see this pattern in the file:

    ```
    RewriteCond %{HTTP_HOST}... dev|stg|ci|ra|uat ...
    ```

    Add the following entry:

    ```
    RewriteCond %{HTTP_HOST}... dev|stg|ci|ra|uat|envname ...
    ```

    Save the updated file and commit to the private gitHub repository.

### Acquia Cloud

* [ ] Login to the [Acquia Cloud Console](https://cloud.acquia.com/app/develop/all) and click on the bostond8 application.
* [ ] Click on the new environment to open the configuration page for that environment.
* [ ] **CRON:** Click on "Scheduled Jobs" and create the following new Job:\
  **Job name:** Envname Site Cron\
  **Command:**\
  `cd /var/www/html/${AH_SITE_NAME}/docroot && drush @bostond8.envname cron -dv &>> /var/log/sites/${AH_SITE_NAME}/logs/$(hostname -s)/drush-cron.log`\
  **Command frequency:** `*/15 * * * * (entered as a string)`
* [ ] **ENVAR:** Click on "Variables" and create variables copied from the `dev` environment.
* [ ] **DOMAIN:** \[optional] Click on Domains and add an edit sub-domain. (this is requested from the LAN team, and should be in the pattern of `d8-envname.boston.gov`. Ask them to set it up the same way that d8-`dev.boston.gov` is set up.  The DNS they create would ideally be private (only to city hall) but they may need to make it on the public DNS servers, which is OK too).

### Single Sign On

The following steps need to be completed to allow single sign on via Ping Federated.

* [ ] aa

### Using the Environment

To use the environment as a Drupal site, you need to attach a branch from the Acquia git repository.  For detailed instructions see [On Demand](./) section.
