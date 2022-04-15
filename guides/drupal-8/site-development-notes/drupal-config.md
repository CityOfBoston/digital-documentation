---
description: >-
  The configuration system for Drupal 8 and 9 handles configuration in a unified
  manner.
---

# Drupal Config

By default, Drupal stores configuration data in its (MySQL) database, but configuration data can be exported to YAML files. This enables a sites configuration to be copied from one installation to another (e.g. dev to test) and also allows the configuration to be managed by version control.

{% hint style="info" %}
**TIP:** Configuration data (aka settings) includes information on how custom and contributed modules are configured.  Think of configuration as the way developers define how the Drupal back-end functions, and what options will be available to content authors.

* _**Configuration**_ is very different to _**content.**_ Content is information which will be displayed to website viewers in Drupal _nodes_. Content is also stored in the database, but is not managed by the configuration system.
{% endhint %}

See this [**Drupal Framework Elements Overview**](https://www.drupal.org/docs/drupal-apis/configuration-api/overview-of-configuration-vs-other-types-of-information)**.**

## Core Configuration Management Utilities

Drupal has a built in configuration management system, along with drush CLI commands to import and export configurations.

1.  Configurations are saved in a folder (the **config sync directory**) on the webserver hosting the Drupal website.  This folder is defined in the settings array `$settings['config_sync_directory']` which is defined in the `settings.php` file.  This folder is defined relative to the `docroot` folder typically outside of the docroot for example:&#x20;

    ```
    $settings['config_sync_directory'] = "../config/default";
    ```
2. `drush cex` exports configurations from the the database into the config sync directory.
3. `drush cim` imports configurations from the config sync directory into the database.
4.  **Module Exclusions:** The Configurations for an entire module can be excluded from both of the `drush cim / cex` processes by defining them in the `$settings['config_exclude_modules']` array in the `settings.php`file. For example:

    ```
    $settings['config_exclude_modules'] = [  'devel', 'syslog' ];
    ```

{% hint style="danger" %}
**WARNING / CARE:**  If you add modules into this list, then they will be removed from the `core.extensions.yml` file during the next config export.  This means these modules will be uninstalled/disabled on any environment in which these configs are imported.

As a rule of thumb - only add modules to this array that you wish to be removed for all environments other than the one you are developing on.
{% endhint %}

{% tabs %}
{% tab title="Drush CLI Config Commands" %}
The Drush CLI is the main CLI utility and is installed and enabled on the CoB Drupal backend.

**`config:delete (cdel)`** \
&#x20;`Delete a configuration key, or a whole object.`

**`config:devel-export (cde, cd-em)`** \
&#x20;`Write back configuration to module's config directory.`

**`config:devel-import (cdi, cd-im)`** \
&#x20;`Import configuration from module's config directory to active storage.`

**`config:devel-import-one (cdi1, cd-i1)`** \
&#x20;`Import a single config item into active storage.`

**`config:diff (cfd)`** \
&#x20;`Displays a diff of a config item.`

**`config:different-report (crd)`** \
&#x20;`Displays differing config items.`

**`config:edit (cedit)`** \
&#x20;`Open a config file in a text editor. Edits are imported after closing editor.`

<mark style="color:blue;">**`config:export (cex)`**</mark> \
&#x20;<mark style="color:blue;">`Export Drupal configuration to a directory.`</mark>

**`config:get (cget)`** \
&#x20;`Display a config value, or a whole configuration object.`

<mark style="color:blue;">**`config:import (cim)`**</mark> \
&#x20;<mark style="color:blue;">`Import config from a config directory.`</mark>

**`config:import-missing (cfi)`** \
&#x20;`Imports missing config item.`

**`config:inactive-report (cri)`** \
&#x20;`Displays optional config items.`

**`config:list-types (clt)`** \
&#x20;`Lists config types.`

**`config:missing-report (crm)`** \
&#x20;`Displays missing config items.`

**`config:pull (cpull)`** \
&#x20;`Export and transfer config from one environment to another.`

**`config:revert (cfr)`** \
&#x20;`Reverts a config item.`

**`config:revert-multiple (cfrm)`** \
&#x20;`Reverts multiple config items to extension provided version.`

**`config:set (cset)`** \
&#x20;`Set config value directly. Does not perform a config import.`

**`config:status (cst)`** \
&#x20;`Display status of configuration (differences between the filesystem configuration and database configuration).`
{% endtab %}

{% tab title="Drupal CLI Config Commands" %}
Drupal is an alternative CLI and is installed and enabled on the CoB Drupal backend.

**`config:delete (cd)`** \
&#x20;`Delete configuration`&#x20;

**`config:diff (cdi)`** \
&#x20;`Output configuration items that are different in active configuration compared with a directory.`&#x20;

**`config:edit (ced,cdit)`** \
&#x20;`Change a configuration object with a text editor.`

<mark style="color:blue;">**`config:export (ce)`**</mark><mark style="color:blue;">` `</mark><mark style="color:blue;">``</mark> \
&#x20;<mark style="color:blue;">`Export current application configuration.`</mark>&#x20;

**`config:export:content:type (cect)`**` ``` \
&#x20;`Export a specific content type and their fields.`&#x20;

**`config:export:entity (cee)`**` ``` \
&#x20;`Export a specific config entity and their fields.`&#x20;

_**`config:export:single (ces)`**` ```_ \
&#x20;_`Export a single configuration or a list of configurations as yml file(s).`_&#x20;

**`config:export:view (cev)`**` ``` \
&#x20;`Export a view in YAML format inside a provided module to reuse in another website.`&#x20;

<mark style="color:blue;">**`config:import (ci)`**</mark><mark style="color:blue;">` `</mark><mark style="color:blue;">``</mark> \
&#x20;<mark style="color:blue;">`Import configuration to current application.`</mark>&#x20;

_**`config:import:single (cis)`**` ```_ \
&#x20;_`Import a single configuration or a list of configurations.`_` ```&#x20;

**`config:override (co)`**` ``` \
&#x20;`Override config value in active configuration.`&#x20;

**`config:validate (cv)`**` ``` \
&#x20;`Validate a drupal config against its schema`

``

**These are unique to the drupal CLI, rarely needed but can be useful for manually creating configs for cusom modules.**

<mark style="color:green;">**`generate:entity:config (gec)`**</mark> \
&#x20;<mark style="color:green;"></mark> <mark style="color:green;"></mark><mark style="color:green;">`Generate a new config entity`</mark>&#x20;

<mark style="color:green;">**`generate:form:config (gfc)`**</mark> \
&#x20;<mark style="color:green;">`Generate a new "ConfigFormBase"`</mark>&#x20;

<mark style="color:green;">**`generate:theme:setting (gts)`**</mark> \
&#x20;<mark style="color:green;">`Generate a setting configuration theme`</mark>
{% endtab %}
{% endtabs %}

### Configuration overrides

It is possible to override configurations in the php files on the Drupal back end.

#### Global Overrides

Normally the configurations a developer will wish to override will be in a xxx.settings.yml file.  This is where settings type configurations are defined and saved by contributed and custom modules.

The strategy to globally override a config setting for the entire Drupal site is to alter the `$config` array in the `settings.php` file.

{% hint style="info" %}
Because the main `settings.php` file can include different settings files for different environments, we can add global overrides to an environment-specific settings.php file to  implement an override for only that environment.&#x20;
{% endhint %}

{% hint style="success" %}
**TIP:** Code in a settings.php file can be conditional, so the override can be made to be conditional on the value of a local (or environment) variable.
{% endhint %}

**Example 1- Core config override:** The `system.maintenance.yml` file contains a `message`key to control text that appears on the site maintenance page when shown.  To override the `message` key set in the `system.maintenance.yml`file, place this in an appropriate settings file.

```php
$config['system.maintenance']['message'] = 'Sorry, our site is down now.';
```

**Example 2- Custom/Contributed Module config override:** The `salesforce.settings.yml` file supplied by the `salesforce` module contains key to authenticate against a salesforce.com account in order to sync data.  To override the `consumer_secret` key set in the `salesforce.settings.yml`file, place this in an appropriate settings file.

```php
$config['salesforce.settings']['consumer_secret'] = '1223425';
```

{% hint style="success" %}
**Override/Secrets Best Practice:**

It is best practice not to save passwords and other secrets (incl API keys) in configuration files, as these will end up in repositories, and could be made public by accident.

Instead, passwords and other secrets should be stored as Environment variables on the Drupal web server,  and then be set in an appropriate `settings.php` file. &#x20;

_**Example:** recaptcha secret key saved as environment variable `bos_captcha_secret`_

```php
$config['recaptcha_v3.settings']['secret_key'] = getenv('bos_captcha_sphpecret_key');
```

This means that passwords and other secrets are saved on the environment to which they apply so there is less (or no) need for environment-specific overrides.

It also means that all secrets are managed the same way, and can be changed on the environment and take effect immediately without needing to redeploy any code.
{% endhint %}

#### Accessing config variables in code

PHP commands retrieve a current configuration settings are as follows:

```php
// Get the site name, with overrides.
$site_name = \Drupal::config('system.site')->get('name');
```

These commands will get the original config value, ignoring any overrides:

```php
// Get the site name without overrides.
$site_name = \Drupal::config('system.site')->getOriginal('name', FALSE);
// Note that mutable config is always override free.
$site_name = \Drupal::configFactory()->getEditable('system.site')->get('name');
```

This information is adapted from this [**Drupal Resource**](https://www.drupal.org/docs/drupal-apis/configuration-api/configuration-override-system), and contains more advanced techniques and discussion.&#x20;

## Contributed Config Management Modules

To assist with configuration management, there are a number of contributed modules.

The contributed modules are generally deployed to help manage situations where different configurations are desired on different environments.

### gitignore

Although this is not a contributed module, the use of `.gitignore` allows a way to prevent configurations from making their way into repositories, and replicating upwards from the local development environments to the Acquia dev/stage/prod environments.

Simply add specific config files (and/or wildcards) to the `.gitignore` file in the root of the repository.&#x20;

Provided the files do not already exist in the repository, they will be ignored by git during commits and pushes from the local repository.

**Example:** .gitignore in repository/project root.

```
...
# ignore settings files with possibly confidential information in them
bos_emergency_alerts.settings.yml
...
```

{% hint style="success" %}
**TIP**: If you don't prefix the entry with any folder paths, then all occurrences of the file will be ignored.  This includes files from config exports (`drush cex`) and also from config\_devel exports (`drush cde` - see below.)
{% endhint %}

### Config Ignore

This module provides **configuration import protection**. If you are concerned that importing certain configurations when using `drush cim` (which is used during a deploy) will overwrite existing configurations on a site, then config ignore will help prevent this.

Specific files to be ignored during an import can be added to the `ignored_config_entities` key of the `config_ignore.settings.yml` file.  This array can also be overridden/extended by altering the `$config['config_ignore.settings']['ignored_config_entities']` array in an appropriate settings file.&#x20;

```php
\\ Just override any config_ignore settings that are already set.
$config['config_ignore.settings']['ignored_config_entities'] =
    [ ... new entries ... ]  

\\ Merge and deduplicate the existing config with these manually added entries.
\\ Caution, this has a (small) performance impact as this merge/dedupe occurs
\\  on each bootstrap of the drupal framework (i.e. each page request/API call)
$config['config_ignore.settings']['ignored_config_entities'] =   
  array_unique(array_merge(    
    $config['config_ignore.settings']['ignored_config_entities'],    
    [ ... new entries ... ]  
  ));
```

{% hint style="info" %}
The `.yml` extension is dropped and wildcards can be used to select entire modules, entities, etc:

`ignored`\_`config_entities:`\
&#x20; `- salesforce.settings`\
&#x20; `- ...`\
&#x20; `- 'core.entity_view_display.node.metrolist_development.*'`
{% endhint %}

{% hint style="info" %}
**Note**: This module only provides protection when `drush cim` is executed.  When `drush cex` is executed, the config\_ignore settings are not considered and a full set of configs is still exported.

If you can't use `$settings['config_exclude_modules']` (because you maybe only want to exclude just the `module.settings.yml` file from a module) then use gitignore to stop it being committed to the repo and deployed.
{% endhint %}

{% hint style="success" %}
**CoB Local Development.**

CoB use `config_ignore` as a fail-safe protection.

Configurations that are set in the production system at runtime (usually settings) via the UI and are therefore different to the config in the `../config/default` folder are added to config\_ignore so that they cannot be imported over the site settings should the files exist in the folder.&#x20;
{% endhint %}

### Config Split

This module provides **configuration separation**. Configurations can be split into different folders and imported/exported independently.

**Drush Command Summary:**

**`config-split:activate`** \
&#x20;**** `Activate a config split.`\
``**`config-split:deactivate`** \
**``**` ``Deactivate a config split.`\
``**`config-split:export`** \
&#x20;`Export only split configuration to a directory.`\
``**`config-split:import`** \
&#x20;`Import only config from a split.`\
``**`config-split:status-override (csso)`** \
&#x20;`Override the status of a split via state.`

Config split can be used to create a number of different configuration sets which can be applied on different environments and/or at different times.  This is an ideal way to control which modules are installed on which environments, and even to provide environment-centric settings (for settings controlled via config).

### Config Devel

This module provides **custom module configuration installation.** If you anticipate your custom module will be used as a "contributed" module on another site - or will be enabled or disabled individually - then you will want to save its configuration into an `install` folder inside the custom module.

**Drush Command Summary:**

**`config:devel-export (cde, cd-em)`** \
&#x20;`Write back configuration to module's config directory.`

**`config:devel-import (cdi, cd-im)`** \
&#x20;`Import configuration from module's config directory to active storage.`

**`config:devel-import-one (cdi1, cd-i1)`** \
&#x20;`Import a single config item into active storage.`

