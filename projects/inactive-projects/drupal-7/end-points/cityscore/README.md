---
description: >-
  Cityscore is a CoB city-performance metric devised by the Mayors office,
  calculated and managed by the analytics team.
---

# Cityscore

Drupal \(via https://www.boston.gov\) is used to provide a public endpoint or micro service which can be used by other departments or external organizations to retrieve current cityscore data for use in their own applications.

{% api-method method="post" host="https://www.boston.gov" path="/rest/cityscore/load" %}
{% api-method-summary %}
Load Cityscore Data
{% endapi-method-summary %}

{% api-method-description %}
This secure endpoint is used by analytics to load and update the current cityscore data.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-form-data-parameters %}
{% api-method-parameter name="payload" type="string" required=true %}
A JSON formatted array of cityscore metric objects.
{% endapi-method-parameter %}

{% api-method-parameter name="api-key" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The endpoint always returns a 200 OK status, with a JSON message in the body.  One of the following messages will be returned:
{% endapi-method-response-example-description %}

```javascript
// Message received and processed correctly.
{
    "status": "success",
    "message": "cityscore updated"
}
// No api-key provided in post form.
{
    "status": "error",
    "message": "error missing token"
}
// No payload provided in post form.
{
    "status": "error",
    "message": "error no payload"
}
// JSON in payload is not corretly encoded.
{
    "status": "error",
    "message": "bad json in payload"
}
// Not all metric objects were loaded.
{
    "status": "error",
    "message": "Not all records saved"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Payload Format.**

```text
[
    {
		"metric_name" : "311 CONSTITUENT EXPERIENCE SURVEYS",
		"score_calculated_ts" : "2018-11-28T12:10:59Z",
		"score_final_table_ts" : "2018-11-28T17:11:01Z",
		"previous_day_score" : null,
		"previous_week_score" : null,
		"previous_month_score" : 5.990000,
		"previous_quarter_score" : 1.063380,
		"score_day_name" : "Tuesday"
	},  
	{
	    ....
	}  
]
```

{% api-method method="get" host="https://www.boston.gov" path="/rest/cityscore/json" %}
{% api-method-summary %}
Get Cityscore Totals
{% endapi-method-summary %}

{% api-method-description %}
This public endpoint returns the latest cityscore indicator value.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "day":0.98,
    "week":0.97,
    "month":1.01,
    "quarter":0.96,
    "date_posted":"01\/16\/2019"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://www.boston.gov" path="/rest/cityscore/html" %}
{% api-method-summary %}
Get Cityscore HTML Table
{% endapi-method-summary %}

{% api-method-description %}
This public endpoint returns an HTML string which contains a cityscore metric table using the CoB style.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cityscore metric table markedup as an HTML table.
{% endapi-method-response-example-description %}

```
<div class="view view-cityscore view-id-cityscore view-display-id-html_cs_table view-dom-id-1ca52cee94c665121a6ce66a2cbf7fe9">
  <div class="view-content">
    <table class="views-table cols-0">
      <tbody>
        <tr>
          <td>
            <tr class="cs__table-row">
              <td>311 CONSTITUENT EXPERIENCE SURVEYS</td>
              <td class="cs__low cs__table--centered">0.95</td>
              <td class="cs__table--centered">-</td>
              <td class="cs__table--centered">1.99</td>
              <td class="cs__table--centered">1.06</td>
            </tr>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
    <div class="view-footer">
      <script>
        <!--//--><![CDATA[// ><!--
        jQuery('table.views-table').append('<thead><tr class="cs__table-header"><th>Topic<th class="cs__table--centered">Day<th class="cs__table--centered">Week<th class="cs__table--centered">Month<th class="cs__table--centered">QTR');
        //--><!]]>
      </script>    
    </div>
  </div>
</div>
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

