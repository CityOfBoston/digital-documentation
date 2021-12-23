---
description: >-
  Iterators is a Certified Trusted Tester by the Department of Homeland Security
  and provides a code inspection-based test approach for determining software
  and web conformance to Section 508 standards.
---

# Working with Iterators

**Visit them at:** [**https://iteratorstesting.com/services/accessibility-testing**](https://iteratorstesting.com/services/accessibility-testing)****

**Trusted Tester Tests:** The Section 508 Trusted Tester program groups the WCAG requirements into related groupings that can be tested (and developed) at the same time. With the help of Iterators the VPAT documentation for boston.gov was created. You can read the [documentation here.](https://docs.google.com/document/d/1vtRusx1qgyiyYx2tUl1hixUTiQqyHQeaVfpECj-HZsc/edit?ts=604fb78c#)

**Test #4) Keyboard and Focus**&#x20;

**Purpose:**

*   Many different types of users decide to use a keyboard to navigate a web page. Some

    examples are screen reader users that will tab through an interface. Other users have

    motor impairments and must control a computer through a switch (such as puffing into a

    straw, etc).
*   People that use a mouse can quickly navigate a website. Additional design features are

    needed to allow the same level of access to people that use a keyboard to navigate a user

    interface.

**Requirements:**

1\) Users should be able to access all functionality by using only the keyboard.

a. First, use a mouse to identify all functionality (buttons, links, etc)

b. Second, try to utilize all of the same functions using the keyboard, mainly using the tab and enter keys.

c. This also applies if popup features are implemented, such as an informational dialog, etc.

2\) Tabbing through a user interface should occur in a logical order that matches the visual pattern on the screen, such as left to right, top to bottom.&#x20;

3\) Tabbing through a user interface should cycle completely through the interface and not be “trapped” in a cycle.&#x20;

4\) Focus is visible when using a keyboard&#x20;

a. Sometimes a blue border is used to show the currently active element when using a keyboard. This should be easily visible with all elements.&#x20;

b. Sometimes using a mouse will also highlight an element or reveal some information. These two methods should be compatible with each other.&#x20;

c. The focus should always be visible and there should not be any invisible elements when using a keyboard.&#x20;

Test #9) Repetitive Content&#x20;

**Purpose**:&#x20;

It is easy to skip over content using a mouse. However, keyboard navigation users must traverse many repetitive elements such as menus.

Requirements:

1\) Identify all areas of a page with repetitive content.

a. A typical example is the navigation menu at the top of web page.

b. Some web pages have multiple sections of different content, such as “news”, “events”, etc. If there are many sections, then a user might like to skip the “news” section and jump to the next section without traverse all of the elements in that section.

2\) Provide a “Skip” link, such as “Skip to Main Content” for each of these repetitive areas. This skip link is typically only visible when a user is using a keyboard to navigate through a web page.&#x20;

**Test #6) Links & Buttons**&#x20;

**Purpose:**

*   Keyboard users should be able to determine the context for any link or button. Often

    users determine the context by visually examining nearby text or graphics.
*   Additionally, some links and buttons cause a change in content on a website which is

    visually observable and also needs to be accessible to keyboard users.



**Requirements:**

*   The major requirement is that accessible and unique descriptions are available for each

    link and button. For example, a set of links to news events might be labeled as “Read

    More…”. However, the user may not be able to determine which news event is

    associated with each link. The link may still be labelled as “Read More…” as long as

    there is also accessible text that provides more description, such as “Read More about the

    City of Boston”.
*   Any new content that becomes visible with a link or button should be identified with

    accessible text before the user clicks on the link or button. An example of this is a

    “Learn More…” button that causes a new modal dialog to be shown. Also, the keyboard

    navigation should move to the new con**t**ent.

**Environments**

* Production: [https://boston.gov](https://boston.gov)
* Test: [https://d8-stg.boston.gov/](https://d8-stg.boston.gov)
* Development: [https://d8-dev.boston.gov/](https://d8-dev.boston.gov), [https://d8-uat.boston.gov/](https://d8-uat.boston.gov), [https://d8-dev2.boston.gov/](https://d8-dev2.boston.gov), [https://d8-ci.boston.gov/](https://d8-ci.boston.gov)
