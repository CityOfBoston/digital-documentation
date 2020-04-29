---
description: Further information about the cityscore micro services.
---

# Knowledge Base

## Drupal Views

There are 2 views which drive the `get` requests for the endpoints.    
The views have a dependency on the `views_php` contributed module.  
These views are defined at: `/admin/structure/views/view/cityscore/edit`.

1. Display: **Cityscore.** This display provides a full Drupal page containing the current cityscore table of metrics.  The "page" URL for this is `/rest/cityscore/html-table`. Because this is a fully themed Drupal page, it is more usual for the endpoint `/rest/cityscore/html` to be used, as that endpoint provides just the `<table>` HTML wrapped in a `<div>` tag.
2. Display: **JSON Output**. This display provides a JSON array with a single object being the current Cityscore total.  The current Cityscore total is the mathematical average \(mean\) of all current non-null metrics. This view is available at the endpoint `/cityscore/totals/latest.json`. Because this is the output from a view, the JSON string is wrapped as an array.  For backwards compatibility the endpoint `/rest/cityscore/json` is used.

## Google Analytics

The endpoints provided at `rest/cityscore/load` , `/rest/cityscore/html`and `/rest/cityscore/json` are all tracked on google analytics.  Filter for pageviews named "/api".



