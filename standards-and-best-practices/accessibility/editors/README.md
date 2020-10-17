---
description: >-
  Updating boston.gov Drupal website with accessibility in mind. When adding
  content by editing/adding html elements, what they need to keep in mind for
  screen readers and people with disability.
---

# Content Editors

**Adding Images**

* When adding images to content, must add `caption` and/or `title` and/or `alt` tags. This is especially important because screen readers reads them. 
* The editor we are using "ckeditor" provides at least one field to enter either a caption, title, or alt tags. If all three fields are provided please enter content for all three fields.
* Please see the "How guide" section on the best ways to add and edit an image.

**Adding Tables**

* Tables summaries must be added to each table created. Editors can use "ckeditor" to add summaries to each table. The `summary` must explain exactly what the table content is about. 
* See the "How to guide" section to learn more about adding table summaries to tables

**Adding Buttons and Links**

* Just like images, links should always have a title if it is to be used as a placeholder or anchor tag. that is it has no content within the `<a></a>` tag.
* Tip for `buttons`: When using `<div>...</div>, <span>...</span>, or <a>...</a>` as buttons always add a `role="button"` attribute to the html tag. The [**button**](https://www.w3.org/WAI/PF/aria/roles#button) role should be used for clickable elements that trigger a response when activated by the user. Adding `role="button"` will make an element appear as a button control to a screen reader. This role can be used in combination with the `aria-pressed` attribute to create toggle buttons. 

```text
<div id="saveChanges" tabindex="0" role="button" aria-pressed="false">Save</div>
```

_The above example creates a simple button which is first in the focus order**.**_

**Audios & Videos**

* Youtube videos:
* HTML videos:
* Other video types:

