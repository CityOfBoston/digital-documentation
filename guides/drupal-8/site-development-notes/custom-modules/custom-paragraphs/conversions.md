---
description: Converting D7 structures to D8
---

# D7 -> D8 Conversion

## Process for creating component modules:

1. Login to the website and go to the paragraphs admin page (`/admin/structure/paragraphs_type`) and delete the paragraph you want to work on
2. Step 1 above may delete some of the field.storage dependencies (field definitions), so just re-import all the bos\_component module config to make sure you get all the shared config back into the database: `lando drush config-import --partial --source=/app/docroot/modules/custom/bos_components/config/install`
3. Create the module scaffolding using drush, for example: `lando drush componetize bos_discussion_topic --components=discussion_topic`
4. Add hook\_theme() to .module file to connect to the paragraph template
5. Copy the corresponding paragraph template from `boston.gov-d8/docroot/themes/preConversion/component` and put it in the scaffolding that the drush command from step 3 created: `docroot/modules/custom/bos_components/modules/bos_discussion_topic/templates`
6. Enable the module: `lando drush en bos_discussion_topic`
7. In the Drupal UI, add the new bundle to the `field_components` paragraph types list for the Test Component Page content type: `/admin/structure/types/manage/test_component_page/fields/node.test_component_page.field_components`
8. Create a test page with the component added to review admin UI and display

### Other helpful commands:

* Importing a single config file:

```
lando drupal config:import:single --file="/app/docroot/modules/custom/bos_components/modules/bos_cabinet/config/install/field.field.paragraph.cabinet.field_contacts.yml"
```

* Exporting database config directly to your module (**Important: **the config file needs to be referenced in your module's info file under the `config-devel` key): `lando drush config-devel-export bos_cabinet`

