---
description: CoB custom modules - usually taxonomy, nodes and paragraphs.
---

# Custom Modules

## Boston.gov Conventions

The following development conventions are being followed in developing boston.gov modules.

### Module naming and location

City of Boston have the following naming and grouping conventions for custom modules:

* [ ] **Content Types**: `module_name` in folder `docroot/modules/custom/bos_components/modules/`
* [ ] **Paragraphs**: `bos_module_name` in folder `docroot/modules/custom/bos_content/modules/`
* [ ] **Taxonomy**: `vocab_module_name` in folder `docroot/modules/custom/bos_vocab/modules/`
* [ ] **Views, blocks, actions etc**: Generally grouped with the main paragraph, node or vocab that uses them.&#x20;
* [ ] **Core:** The module bos\_core defines core services such as REST, Drush and any views or blocks that are used globally (by more than one node, paragraph or vocabulary).
* [ ] **Theme:** boston.gov has two custom themes; `bos_theme` for all UX theming, and `bos_admin` for all content editor UX theming.
* [ ] **Profile:** boston.gov does not have a custom profile.  Drupal default minimal is used.
* [ ] **Custom:** Custom modules containing backend code are named appropriately and added to the folder `docroot/modules/custom.`

### Twig Templates

Templates for the component should be saved in:

```
module_name/templates
```

To add a customized template, select a suggestion for the base (node, field, region etc), then

1. Save the template in the folder above
2.  In the `module_name_theme()` hook in `module_name.module` add the following:

    ```php
    function module_name_theme() {
      return [
        'template__suggestion_name' => [
          'base_hook' => 'region',
          'path' => 'modules/custom/bos_components/modules/module_name/templates',
          'template' => 'template--suggestion-name',
        ]
      ];
    }
    ```
3.  If a new suggestion is needed, then add the following:

    ```php
    function module_name_theme_suggestions_XXX_alter(array &$suggestions, array $variables) {
      // Add a new suggestion based 
      if ($variables['some_element'] == "some_condition") {
        $suggestions[] = "new__suggestion";
      }
    }
    ```

    Where XXX is the appropriate entity type (node, field, region, etc etc) to add a suggestion to.&#x20;

### Theme overriding

Wherever possible, the style provided from the patterns library should be used.  In practice this means that boston.gov can be styled by a Drupal developer ensuring that the twig template files provide HTML structured and given classes that the patterns library expects.

Should the need arise, then the patterns library style sheets can be overridden.  Typically this is done at the module level, although if multiple modules will use the override, consider placing it in the `bos_theme` theme.

To add overrides,&#x20;

1. Create the style sheet `module_name.css` and appropriate markup in the relevant template (see above section),
2.  Save the stylesheet in:&#x20;

    ```
    module_name/css
    ```
3.  Update (or create) the `module_name.libraries.yml` file with the following:

    ```yaml
    something.override:
      css:
        theme:
          css/module_name.css: {}
    ```
4.  Using a `module_name_preprocess_HOOK()` hook in `module_name.module`attach the css where and only when it is required. For example:

    ```php
    function module_name_preprocess_HOOK(&$variables) {
      $variables['#attached']['library'][] = "bos_modulename/something.override";
    }
    ```

### Using JavaScript

Wherever possible, JavaScript should not be used on boston.gov.  This is to maintain compatibility with as many browsers as possible, and to maximize accessibility for screen readers etc.

Should the need arise, then a JavaScript library can be created and deployed.  Typically this is done at the module level, although if multiple modules will use the override, consider placing it in the `bos_theme` theme.

To add overrides,&#x20;

1. Create the JavaScript library `module_name.js`,
2.  Save the library in:&#x20;

    ```
    module_name/js
    ```
3.  Update (or create) the `bos_modulename.libraries.yml` with the JavaScript directive - for example you could add the following:

    ```yaml
    something.javascript:
      js:    
        js/module_name.js: {}
        js/module_name1.js: { browsers: { IE: 'IE 9', '!IE': FALSE  } }
        js/module_name2.js: {attributes: {id: "component_nav_script"}}

      dependencies:
      - core/jquery
    ```
4.  Using a `bos_modulename_preprocess_HOOK()` hook in `bos_modulename.module`attach the JavaScript library where and only when it is required. For example:

    ```php
    function module_name_preprocess_HOOK(&$variables) {
      $variables['#attached']['library'][] = "module_name/something.javascript";
    }
    ```

### Site and Module Configuration

Drupal 8 defines settings and configurations in YML files with the actual "current" settings and configurations being stored in the Drupal (MySQL) database. &#x20;

{% hint style="info" %}
When the website is deployed or the web-server is restarted, configurations are re-read from the database.  To reload the configuration and settings from yml files requires a manual (usually `drush`) process to be run by a developer.
{% endhint %}

{% hint style="info" %}
Clearing the sites caches causes cached configurations and settings to be replaced with values from the database.  Clearing caches does not reload yml files.
{% endhint %}

#### Importing YML configuration into the database.

YML files in a modules`docroot/modules/custom/ ... /module_name/config/install` folder will be imported into the database when a module is first installed.&#x20;

YML files in the `docroot/../config/default/` folder will be imported into the database when the configuration is imported via the Drupal UI, or the `drush` command.

```
drush cim
```

#### Exporting all database settings to YML files.

Current (run-time) settings and configurations in the database can be exported to the `docroot/../config/default/` folder via the Drupal UI, or the `drush` command.

```
drush cex
```

#### Defining module-specific configurations using config\_devel.

If the `config_devel` module is enabled then a modules configuration can be exported to the modules `config/install` folder.

The dependent configurations are defined in the `module_name.info.yml` file as follows:

```yaml
config_devel:
  - module_name.settings
  - ...
```

To export these configurations to the `config/install` folder use the following drush command:

```
drush cde module_name
```

#### Configuration considerations

* Modules should try to reuse `field.storage.entity-type.field_name` configurations wherever possible.
* `field.storage.entity-type.field_name` configurations should be: \
  1\. saved in the modules parent module (e.g. `bos_components` or `bos_content`)to enable sharing, and\
  2\. added to the parents `config_devel` section of the `.info.yml` file.

