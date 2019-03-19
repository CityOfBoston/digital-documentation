---
description: Developer notes for content type (node) design and implementation.
---

# Custom Content Types

## Content Type Module Components

Modules can define multiple content types \(nodes\) grouped by similar function.

A good example module can be found at:

```text
docroot/modules/custom/bos_content/modules/article
```

### Module folder

Module naming convention is to call the module `module_name`. The "**module\_name**" should be indicative of the node/s contained within the module.

{% hint style="success" %}
Sub-pages in this section assumes an example module is to be named `module_name` and therefore the module folder would be:

```bash
docroot/modules/custom/bos_content/modules/module_name
```
{% endhint %}

