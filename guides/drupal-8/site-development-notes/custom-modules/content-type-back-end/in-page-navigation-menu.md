---
description: >-
  Custom nodes deployed in boston.gov have a navigation menu which sits below
  the introduction text on each page.
---

# In-page Navigation Menu

{% hint style="info" %}
The in-page menu requires the node to embed paragraphs, the node--_xxxx_.html.twig to contain a &lt;div&gt; and for each embedded paragraph to have a key field.
{% endhint %}

## Requirements

### Node: field\_components.

If the node has components \(paragraphs\) embedded, then the node will have a field called `field_components` and this field will be of a type `Entity reference revisions`.  The field will allow only paragraphs, and will specify the paragraph types that are allowed on the node.

### Paragraph: field\_short\_title.

To enable in-page navigation, each paragraph must have a \(text field\) `field_short_title`, and to reduce confusion for content editors, that field should be named "**Navigation Title**".  

{% hint style="success" %}
To make the menu look nice and work well on mobile devices, content editors and authors should be encouraged to keep the content added to the **Navigation Title** to 20 chars or less.
{% endhint %}

### Node: template.

To enable the in-page navigation menu, the nodes template should include the following:

```text
{% if navOutput %}
    <div class="sub-nav-trigger drawer-trigger">
        <div class="sub-nav-chevron">
            {{ sub_nav|raw }}
        </div>
        Page Sections
    </div>
    <nav class="topic-nav topic-nav__left">
        <a name="section-nav"></a>
        {{ navOutput }}
    </nav>
{% endif %}
```

{% hint style="info" %}
This block should ideally be located below the title and intro-text sections.
{% endhint %}

* [ ] The div `drawer-trigger` block is required to collapse menu for mobile devices
* [ ] The div `topic-nav` is required for menu on desktop devices.

## How it works

### Expected behavior 

When there is more than 1 paragraphs embedded in a nodes web page, an in-page navigation menu should appear on the page.  The menu should be styled from the patterns library.

**UX Desktop:** When the page first loads, the menu should display above the fold.  As the user scrolls down the page, the menu should collapse into a fixed toolbar at the top of the page, below the seal menu with the seal retracted.  Theme should come from patterns.

**UX Mobile**:   Menu should appear as a collapsed set of drawers with a chevron icon to expand. Css from patterns controls the collapse across the responsive page width.

In either UX, when the user clicks on the menu, the page should scroll smoothly down to the correct paragraph display on the webpage.

### Locating and creating menu

The twig template \(e.g. `node--xxx.html.twig`\) for the node is responsible for locating the menu on the node.  The code required is described above.

On-page menu elements are rendered from the `bos_theme_preprocess_node()` and `bos_theme_preprocess_field()`  hooks in `bos_theme.theme` found in `/themes/custom/bos_theme/`.

### UX

The page click and scrolling is provided by `component-navigation.boston.js` which is found in `/themes/custom/bos_theme/js/`.

### Making paragraphs compatible

To make a paragraph include itself in the in-page navigation menu, it just needs to contain a text field named `field_short_title` \(and for that field to be included in the display being used on the node\).

