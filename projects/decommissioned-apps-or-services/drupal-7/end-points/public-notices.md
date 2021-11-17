---
description: >-
  Public Notices are contributed directly into Drupal by Content-Editors.  This
  endpoint provides filtered and sorted lists of Public Notices.
---

# Public Notices

{% api-method method="get" host="https://www.boston.gov" path="/api/v2/public-notices" %}
{% api-method-summary %}
Public Notices.
{% endapi-method-summary %}

{% api-method-description %}
This endpoint provides a listing of the next 30 public notices which started in the last 3 hours or will start at some time in the future.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Listing of filtered public notices.
{% endapi-method-response-example-description %}

```
[
    {
        "is_testimony":"1",
        "id":"54401",
        "testimony_time":"",
        "title":"<a href=\"/public-notices/54401\">Zoning Board of Appeal hearing</a>",
        "location_name":"Boston City Hall",
        "location_room":"Room 801",
        "location_street":"1 City Hall Square",
        "canceled":"0",
        "notice_date":"January 15, 2019",
        "notice_time":"9:30am",
        "field_drawer":"<li>\n  <strong>\n  APPROVAL OF HEARING MINUTES:</strong>\n  \n  <p>December 11, 2018</p>\n  </li>\n",
        "body":"",
        "posted":"01/10/2019 - 9:06am"
    },
    {
      ....
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Response Notes:**

* All response fields are _**always**_ strings.
* Blank \(empty\) fields are still provided as key:value pairs, with the value being an empty string \(""\) - the API does not use the keyword "null".
* Title, body and field\_drawer may contain basic HTML mark-up.



