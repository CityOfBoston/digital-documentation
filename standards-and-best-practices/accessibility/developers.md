---
description: What developers should take into consideration when writing code.
---

# Developers

**Testing**

1. Install ANDI: [https://www.ssa.gov/accessibility/andi/help/howtouse.html](https://www.ssa.gov/accessibility/andi/help/howtouse.html)

**Adding Tables**

* Add "`summary`" to table tag. A **summary** conveys information about the organization of the data in a table and helps users navigate it.
* Add "`scope`" to tables with headers. The `scope` attribute can be set to `row` or `col` to denote that a header applies to the entire row or column, respectively.
* Add "`captions`". A **caption** functions like a heading for a table. Most screen readers announce the content of captions. 

```text
<table summary="some summary here">
  <caption>Concerts</caption>
  <tr>
    <td></td>
    <th scope="col">Date</th>
    <th scope="col">Event</th>
    <th scope="col">Venue</th>
  </tr>
  <tr>
    <th scope="row">09:00 - 11:00</th>
    <td>12 Feb</td>
    <td>Waltz with Strauss</td>
    <td>Main Hall</td>
  </tr>
  […]
</table>
```

**Adding SVGs**

* If it is used as an `img`, must add `title` attribute to it.
* If `svg` is use as a button, must add a `tabindex` attribute
* 
```text
<label for="s-tr" title="Search" class="nv-h-l-a nv-h-l-a--k nv-h-l-a-ic" id="searchIcon" tabindex="0">
   <svg id="Layer_2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 41"><title>Search</title>
     <path class="nv-h-l-a-i" d="M24.2.6C15.8.6 9 7.4 9 15.8c0 3.7 1.4 7.2 3.6 9.8L1.2 37c-.8.8-.8 2 0 2.8.4.4.9.6 1.4.6s1-.2 1.4-.6l11.5-11.5C18 30 21 31 24.2 31c8.4 0 15.2-6.8 15.2-15.2C39.4 7.4 32.6.6 24.2.6zm0 26.5c-6.2 0-11.2-5-11.2-11.2S18 4.6 24.2 4.6s11.2 5 11.2 11.2-5 11.3-11.2 11.3z"></path>
   </svg>
</label>
```

**iFrames**

* iFrames needs to have a `title` attribute 

**images vs background images**

* Images must have an `alt` attribute
* Images can also have a `title` attribute
* If an image gives a better understanding or context to a content, that images should not be a background image but an actual image.  

**Summary**

Developers can emulate links with other elements, such as `<div>` or `<span>` elements and JavaScript click listeners. But, these kinds of emulated links need care. Developers wishing to emulate links must include the following:

* Add `tabindex=”0”` so that the link becomes keyboard focusable
* Add `role=”link”` so that assistive technology recognizes the element as a link
* Add the styling cursor: pointer so that mouse users will recognize the element as a link.
* Same applies to html elements used as buttons instead of the actual `<button>` or `<input>` element. 

```text
<!-- Link example -->
<span role=”link” tabindex=”0” style=”cursor:pointer;text-decoration:underline;color:blue;”>This is an emulated link</span>

<a href="https://boston.gov">Click Here</a>

<a class="subnav-anchor" id="stay-connected" data-text="Stay Connected" href="javascript:void(0)" title="This is an anchor"></a>

<!-- Button example -->
<div id="saveChanges" tabindex="0" role="button" aria-pressed="false">Save</div>

<button id="saveChanges">Save</button>
```

For example, the markup for an accessible emulated link might look like the following:

To avoid needing to implement the above, developers should prefer to use the `<a>` tag instead.

**Note**: If using role="button" instead of the semantic `<button>` or `<input type="button">` elements, you will need to make the element focusable and have to define event handlers for [`click`](https://developer.mozilla.org/en-US/docs/Web/Events/click) and [`keydown`](https://developer.mozilla.org/en-US/docs/Web/Events/keydown) events, including the Enter and Space keys, in order to process the user's input. See the [official WAI-ARIA example code](https://www.w3.org/TR/wai-aria-practices/examples/button/button.html).

