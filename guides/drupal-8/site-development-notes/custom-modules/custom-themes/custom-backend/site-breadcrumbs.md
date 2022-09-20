---
description: >-
  Breadcrumbs are an informative device which appear on many pages on the site.
  Breadcrumbs provide the user a sense of location within the site and a way to
  logically navigate back to the homepage.
---

# Site Breadcrumbs

A breadcrumb is an ordered collection of crumbs, with each crumb having a title and a link.

### Drupal Core Breadcrumb Functionality

Drupal has a built-in breadcrumbs methodology, which will attempt to build out a pathway based on the URI (e.g. `/departments/housing/metrolist`) defined by the pages (i.e. nodes) _**URL Alias**_.&#x20;

{% hint style="success" %}
_It does not matter if the **URL Alias** is set manually or automatically, the value shown in the back-end editor form once the node is saved is used to build out the breadcrumb._
{% endhint %}

The Drupal core process creates the breadcrumb by scanning the path represented by the URI, and testing if a local page exists for each path element.  It stops adding crumbs when a path element does not resolve. &#x20;

**FOR EXAMPLE** an article is created with a URI (as defined in its URL Alias):\
&#x20;  `/departments/housing/boston/housing-information-in-boston.`

When the page is rendered, Drupal scans the articles URI and&#x20;

* if we have a breadcrumb setting which stipulates that the homepage should always be shown as the first crumb, then a crumb of `home` with a link to `https://site` is created, then
* checks if `/departments` is a valid URI. `https://site/departments` is a valid URI, so it creates a crumb of "departments" with a link to `https://site/departments` , then
* checks if `/departments/housing` is a valid URI. `https://site/departments/housing` is a valid URI, so it creates a crumb of "housing" with a link to `https://site/department/housing` , then
* checks if `/departments/housing/boston` is a valid URI. `https://site/departments/housing/boston` is NOT a valid URI - there is no page with that name on `https://site` so the breadcrumb scanner stops evaluating at this point, but
* if we have a breadcrumb setting to display the actual page in the breadcrumb then a final crumb of `housing information in boston` is added, with no link (because this is the page showing).

The final breadcrumb in this instance would be \
&#x20;   HOME > DEPARTMENTS > HOUSING > <mark style="color:red;">HOUSING INFORMATION IN BOSTON</mark>\
with links on the first 3 crumbs.

{% hint style="info" %}
When evaluating if a page exists on the site, Drupal only considers **URL Aliases** and does not check **URL Redirects**.\
So in the example above, the `boston` crumb/link still would not appear in the breadcrumb even if a `place_profile` page for Boston existed with the URL Alias of `/places/boston` and a URL Redirect for `/departments/housing/boston`.
{% endhint %}

### CoB Theme extension for breadcrumbs

Where Drupal core cannot build out its own breadcrumb trail, there is some additional custom code intended to help make a logical breadcrumb.

{% hint style="info" %}
<mark style="color:orange;">**The custom breadcrumb code only functions when it determines that Drupal has not built out the entire breadcrumb.**</mark> &#x20;

<mark style="color:orange;">**If Drupal has been able to build out all parts of the URI path, then the Drupal breadcrumb is used.**</mark>
{% endhint %}

#### Scans Redirects

The custom code scans URL redirects as well as URL Aliases when building out the breadcrumbs.

{% hint style="danger" %}
**Care:** Redirects which are manually made on the page `admin/config/search/redirect` are usually considered "external" by default.  Breadcrumbs which use an external link may behave unexpectedly when clicked.&#x20;

**Example:** the breadcrumb on d8-dev.boston.gov may open a page on www.boston.gov when clicked.

**Solution:** Do not create redirects for internal (i.e. Drupal hosted) pages on in the `admin/config/search/redirect` page.  Instead create redirects using the redirect function on the "advanced" tab of the editor form for a page.
{% endhint %}

#### Custom Replacements

Some URI paths are hard-coded to build specific breadcrumbs. &#x20;

For example pages which have a URI path starting with `government/cabinet` . The custom code ignores the "government/cabinets" part of the path and then build the breadcrumb from the remainder of the path.

{% hint style="info" %}
The custom breadcrumb object is **built** here: `bos_theme/bos_theme.theme::bos_theme_preprocess_breadcrumb()`
{% endhint %}

{% hint style="info" %}
The breadcrumb is **styled** here: `bos_theme/templates/navigation/breadcrumb.html.twig`
{% endhint %}
