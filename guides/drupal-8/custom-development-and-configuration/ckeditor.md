---
description: >-
  Drupal 9 (our current install) uses CKEditor 4, when we move to Drupal 10, it
  uses CKEditor 5. CKEditor 5 is currently installed in core/modules, but not
  used.
---

# CKEditor

We currently have 2 or more versions of CKEditor we use plus extension of the plugin in 2 other components.

```
/boston.gov-d8/docroot/core/modules/ckeditor

/boston.gov-d8/docroot/core/modules/ckeditor5

/boston.gov-d8/docroot/modules/contrib/ckeditor

Dependencies/Dependent

/boston.gov-d8/docroot/modules/contrib/ckeditor

/boston.gov-d8/docroot/modules/contrib/editor_advanced_link

Custom components

/boston.gov-d8/docroot/modules/custom/bos_linkit_extension

/boston.gov-d8/docroot/themes/custom/bos_theme/js/cob_ckeditor.boston.js

CKEditor versions are in:

core/modules 
or the version in:

modules/contrib
But the one thing to consider is that we are still on Drupal 9 right now, when we move to Drupal 10, we will need to switch from CKEditor 4 to 5, because Drupal 10 only works with CKEditor 5 which is currently installed in core/modules, but I think it is not currently used.

So far it looks like the only version been used is the one in:

modules/contrib
We will have to decide which ckeditor to use. But a lot of test needs to be to avoid breaking another module/component that may of the off chance be using the one we will delete.
```

### Drupal 9

Our current Drupal version D9 uses the CKEditor 4 in modules/contributor folder.&#x20;

### Drupal 10

Once we upgrade to Drupal 10, will need to move from CKEditor 4 to 5. That is because Drupal 10 does not use CKEditor 4.

Samples of CKEditor 5 we can explore to integrate/use can be found here:

&#x20;[CKEditor 5 Demo](https://ckeditor.com/ckeditor-5/demo/)

## Related Jira tickets

* [DIG-2204](https://bostondoit.atlassian.net/browse/DIG-2204)
* [DIG-2176](https://bostondoit.atlassian.net/browse/DIG-2176)
