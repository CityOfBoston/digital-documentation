---
description: >-
  Breadcrumbs are an informative device which appear on many pages on the site.
  Breadcrumbs provide the user a sense of location within the site and a way to
  logically navigate back to the homepage.
---

# Site Breadcrumbs

Drupal has a built-in breadcrumbs methodology, which will attempt to build out a pathway based on the URI (e.g. `/departments/housing/metrolist`) defined by the pages (i.e. nodes) _**URL Alias**_. \
_It does not matter if the URL Alias is set manually or automatically, the value shown in the back-end editor form once the node is saved is used to build out the breadcrumb._

A breadcrumb is an ordered collection of crumbs, with each crumb having a title and a link.

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

Where Drupal core cannot build out its own breadcrumb trail, there is some additional custom code intended to help make a logical breadcrumb.

{% hint style="info" %}
The custom breadcrumb object is **built** here: `bos_theme/bos_theme.theme::bos_theme_preprocess_breadcrumb()`
{% endhint %}

The custom breadcrumb code catches the situation where Drupal has only been able to generate a breadcrumb with the `Home` crumb.  The code does similar URI scanning as the Drupal core activity, but it does recognize URL Redirects as well as URL Aliases.

{% hint style="info" %}
The breadcrumb is **styled** here: `bos_theme/templates/navigation/breadcrumb.html.twig`
{% endhint %}

