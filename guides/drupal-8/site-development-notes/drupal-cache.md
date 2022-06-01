---
description: Caching considerations for Drupal with Acquia
---

# Drupal Cache

## Layered approach to caching.

City of Boston use Acquia to host all non local (docker) servers on our [deployment workflow](https://docs.boston.gov/digital/guides/drupal-8/continuous-deployment-process). &#x20;

### Varnish

Acquia's servers are contained within an [Acquia Cloud](https://cloud.acquia.com/app/develop/applications/5ad427f5-60d6-48fd-983e-670ddc7767c4) subscription and implement a [Varnish](https://varnish-cache.org/) cache outside the load-balancer, as [described here](https://support.acquia.com/hc/en-us/articles/360005853313-Introduction-to-Varnish).

### Drupal Core Cache

The release of Drupal 8 contains a [rebuilt cache strategy](https://www.drupal.org/docs/8/api/cache-api) using "[Tags](https://www.drupal.org/docs/8/api/cache-api/cache-tags)".  Drupal 7's cache expired items based on a lifetime for that item.  Drupal 8 introduces another option called cache invalidation. This is where you set the cache lifetime to be permanent and invalidate (purge) that cached item when its no longer relevant. Drupal 8 does this by storing metadata about the cached item. Then, when an event occurs, such as an update on a node, the metadata can be searched to find all cache items that contain computed data about the updated node, and can be invalidated.

### Memcache

[Memcache](http://memcached.org/) (for the purposes of this summary document) can be considered to be a low-level cache which optimizes caching by saving more dynamic process responses to memory.  The principal value is to minimize requests between the Drupal kernel and MySQL for queries that are run multiple times during bootstrap and page requests.&#x20;

_**Memcache is not used on boston.gov (at this time).**_

### Discover cache activity.

You can inspect the headers of requests to a webserver to see if varnish is enabled, and if content was served from the Varnish and/or Drupal caches.

This terminal command will return the headers from a request to a URL:

```bash
curl -IXGET <URL>
```

Examples:

```bash
# Production website.
curl -IXGET http://www.boston.gov

# Acquia cloud development environment.
curl -IXGET http://d8-dev.boston.gov

# Local website (if entered on your local development machine).
curl -IXGET http://boston.lndo.site
```

### Cache layer highlights by layer.

{% tabs %}
{% tab title="Varnish" %}
* Is "passive" caching: Varnish is not aware of the origin of html content it serves/caches.
* Is outside of the Acquia load-balancers and is the first cache a user request hits.
* Does not cache content for authenticated users.
* Is fully independent from the Drupal kernel, and therefore is decoupled from Drupal -except for a purge module provided by Acquia which manipulates a Varnish API.\
  \- [https://docs.acquia.com/resource/caching/purge/](https://docs.acquia.com/resource/caching/purge/) (**Beware:** notes are for Drupal 7)\
  \- [https://www.drupal.org/project/acquia\_purge](https://www.drupal.org/project/acquia\_purge)
* [Drupal documentation](https://support.acquia.com/hc/en-us/articles/360005853313-Introduction-to-Varnish#whathappenswheniupdatemysite%E2%80%99scontent?) says in Acquia Cloud, **pages are cached for 2 minutes** by default.
* Varnish will accept caching instruction from a web-page headers, so we use [Advanced Page Expiry](https://www.drupal.org/project/ape) (APE) in drupal to send specific cache instructions to Varnish. The default caching time (set by APE) for CoB drupal pages is 4 weeks (i.e. overrides default 2minutes with 4 weeks!).
* On boston.gov, the Acquia Purge module is configured to remove entities (pages) from the Varnish cache as they are updated by content editors in Drupal.  This invalidation process uses queues in Drupal.  The Drupal queue processor is triggered by cron and runs until the queue is exhausted. &#x20;

{% hint style="info" %}
**On `production` cron runs every 5 minutes,** \
****_so (if there is no active queue) it could take up to 5 minutes for content changes to appear._&#x20;

On**`stage`and`develop`cron runs every 15 minutes.**
{% endhint %}
{% endtab %}

{% tab title="Memcache" %}
* Acquia provide the `memcahed` libraries on its environments, and will configure special memory allocations for memcache on request.
* Memcache modules are not enabled on the City of Boston Drupal 8 environments.
{% endtab %}

{% tab title="Drupal" %}
**Images:**

**Static Content**: (typically web-pages built from a Drupal content type)

* Drupal entities are cached using [tags](https://www.drupal.org/docs/8/api/cache-api/cache-tags).
* Drupal caching is managed by the Drupal kernel and the [advanced\_page\_expiration](https://www.drupal.org/project/ape) module (APE).
* When an entity (bit of content) is updated in Drupal its tags are invalidated.  Pages which  use that content (and which are are already cached by Drupal) are also invalidated.  Next time that page is requested a rebuild/regeneration and re-cache occurs within Drupal.
  * When a page is invalidated in Drupal, Varnish is notified and the page is also invalidated in the Varnish cache.
* Because Drupal caching and invalidation is now so effective, the page-expiry for nodes should be set to a large value (> 1 month).  This is done in the APE configuration.

**Dynamic Content**: (typically REST end-points and web-pages built from, or containing Drupal views)

* Views by default honor the tag generation and invalidation process whereby a view is cached with a tag, but the view invalidation model is not very refined (to refine the invalidation of views tags consider [views\_custom\_cache\_tags](https://www.drupal.org/project/views\_custom\_cache\_tag) module - but (as of version 8.x.1.1) custom coding is required to implement). If a view is based upon the entity type node, then any change that invalidates a node tag will also invalidate the view.  Although this causes (potentially) unnecessary invalidation of views, it is an effective way to ensure current content is returned from a view. If the view display is a page, then the invalidation the views does bubble up to Varnish (provided it is using a tag-based cache strategy).
* Views can be given a lifetime, and set to expire a certain time after the last time the views underlying query was run.  As I understand, with time-based caching there is no invalidation of the node, but as the content expires it will be re-cached by Drupal using the traditional (Drupal 7) method.  The page containing the view should be set to expire at a relatively short period (in APE) - around the same time value as the view cache expiry.  Unless told otherwise Varnish expires the page after 2 minutes.
* REST endpoints should be given an expiry in APE.
{% endtab %}
{% endtabs %}

{% hint style="success" %}
The Varnish cache performs 2 functions, one intended and one somewhat unintended.

1. Reduces load on the application server (i.e. webserver), but also
2. The cache will continue to serve cached pages even if the application server (webserver) is down or otherwise unavailable. Any cached pages in varnish will continue to be served until the pages expire in the cache.\
   **Note:** Not all pages are cached, and authenticated sessions are not cached.
{% endhint %}
