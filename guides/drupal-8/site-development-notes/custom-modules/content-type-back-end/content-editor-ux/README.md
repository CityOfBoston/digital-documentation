---
description: Notes on bos_admin theme for UX when adding content via admin pages.
---

# Content Editor UX

## Grouping on Form Display

To keep a clear and clean editor experience which uniform across the site, the form display configuration for nodes will contain groups.

There will be a root (parent) group of type `tabs`. This group will contain child groups of type `tab`.  Each tab group will contain the nodes fields.

{% hint style="success" %}
**Recommended Grouping Layout:**\
1\. _Required_: Create a parent `tabs` group called `group_main` (the name is not important).\
2\. Create child `tabs` groups with the following layout:\
&#x20;    \- **Basic Information**: Contains custom fields required by the new content-type,\
&#x20;    \- **Sidebar Components:** Single `Entity reference revisions`field for sidebar paragraphs,\
&#x20;    \- **Components:** Single `Entity reference revisions` field for main page paragraphs,\
&#x20;    .. then other `tab` groups is needed (try to minimise if possible).
{% endhint %}

The use of further nested groups is discouraged, except for grouping which occurs within paragraph components that are exposed in **Components** or **Sidebar Components** tabs.&#x20;

If other groups are required to help clarify the form display, they should be `details` type groups, and should be set to be collapsible, and be collapsed by default.

{% hint style="warning" %}
IMPORTANT: \
For site consistency, ensure any and all `Entity reference revisions` (i.e. paragraphs) on the node are set to **`"Paragraphs (EXPERIMENTAL)"`** in the form display.
{% endhint %}

## Admin Theme

The `bos_admin` theme makes some changes to the node administration forms.

### Settings collapsed into advanced vertical tab

Config settings provided by drupal core and drupal contributed modules are moved into a `tab` called advanced, and are set as children of the `tabs` group as defined above.  \
This manipulation is done in the hook`bos_admin_form_alter()`found in `bos_admin.theme` file at `themes/custom/bos_admin` .&#x20;

### Moderation/workflow collected and move to right sidebar

The moderation state, revision log note and save / preview / delete buttons are grouped together in a details group and moved to the right sidebar area of the administration form.\
&#x20;This manipulation is done in the hook`bos_admin_form_alter()`found in `bos_admin.theme` file at `themes/custom/bos_admin`

### Content moderation workflows and UX

{% content-ref url="content-moderation.md" %}
[content-moderation.md](content-moderation.md)
{% endcontent-ref %}
