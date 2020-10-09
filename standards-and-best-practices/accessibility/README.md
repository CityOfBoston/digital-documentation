---
description: >-
  Web accessibility means that websites, tools, and technologies are designed
  and developed so that people with disabilities can use them.
---

# Accessibility at COB

**What to look for**

* Keyboard `focus` on focusable elements
* Tab order
* Label tags
* Title attributes

**Keyboard access**

* All "clickable" elements must be keyboard accessible
* Likewise all hover/focusable elements must have alternate keyboard focus matching the mouse hover

**Colors and fonts**

* Text colors should be tested for contrast. Make sure there is good color contrast between the text and the background
* A recommended minimum **font** size is 16 px. Use bold to add emphasis rather than italics or UPPERCASE, but use it sparingly!
* 
**Images vs background images**

* Use background images only if necessary
* All images must have an alt tag
* Actual images must be use if it gives content more meaning or is the only content.

**HTML Structure**

* Content will be read from left to right, therefore html should be written out the way the keyboard will tab through content
* Input elements should have labels. They can hidden if not part of the design, but needs to be there for screen readers
* All HTML should have proper `aria` attributes
* Lists should have the proper `role` attribute

**Titles, captions, and summaries**

* Iframes
* Tables
* Images

**Links and buttons**

* Production: [https://access.boston.gov](https://access.boston.gov)
* Test: [https://access-boston-test.digital-staging.boston.gov/](https://access-boston-test.digital-staging.boston.gov/)
* Development: [https://access-boston-dev.digital-staging.boston.gov/](https://access-boston-dev.digital-staging.boston.gov/)

