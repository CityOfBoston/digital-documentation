---
description: Technical and project documentation for charts on boston.gov.
---

# Charts on boston.gov

We use the [VegaLite](https://vega.github.io/vega-lite/) and [Vega](https://vega.github.io/vega/) libraries to create our charts on boston.gov. Vega is a wrapped up and easier to use/understand version of Vega. These libraries let users define/create a chart using a JSON schema. 

We wrap this library up as a web component that is stored in [Fleet](fleet-pattern-library.md), our patterns library. The `<cob-chart>` component takes a JSON object as input in a Vega or VegaLite schema as input and creates a chart. 

In putting these chart on boston.gov, we put them within text components in an effort to bring context to the data and information the chart is displaying. The [FY20 Budget Website](https://www.boston.gov/departments/budget/fy20-budget) has good examples of how charts should be used with narrative explanation. 

#### Good Reads

* [How Vega works Observable Notebook](https://observablehq.com/@vega/how-vega-works)
* [Guide to Axes & Legends in Vega](https://observablehq.com/@vega/a-guide-to-guides-axes-legends-in-vega)

## Putting a chart on boston.gov

In general, content on boston.gov should be at or below an 8th grade reading level. In an attempt to be in accordance with this and keep our site as easy-to-understand as possible, we push for simple and straight forward charts \(e.g. bar charts, line charts, and sometimes pie charts if we have to\). 

Coordination with both the boston.gov Content Manager \(for chart placement\) and the Analytics Team \(for chart data\) will be required to get a chart on boston.gov. 

### Getting the data

Vega/VegaLite can read in data from a publicly available url. We leverage this functionality so that we can set up separate automation practices for the data charts on boston.gov display. 

{% hint style="info" %}
When putting a chart on boston.gov, coordination with the Analytics Team is necessary in order to set up ETL workflows/processes that will \(ideally\) prevent the data shown in the chart on boston.gov from going stale.
{% endhint %}

For testing purposes, you could upload a csv as a [Github Gist](https://gist.github.com/) and use that url, but when things move to production, all data should be pulling from an s3 bucket.

#### Using s3

The Analytics Team has the ability to create s3 buckets that can store completely public csv files. Once a file has been loaded to a public bucket, you can supply the "Object URL" to Vega. 

![Location of the &quot;Object URL&quot; in s3. ](../.gitbook/assets/image%20%283%29.png)

Storing data for charts on s3, means we can set up separate workflows that will automatically update the data on the chart. For example, for the [FY20 Budget website](https://www.boston.gov/departments/budget/fy20-budget), we set up a workflow in [Civis](https://app.gitbook.com/@boston/s/analytics/guides/civis/introduction) \(the Analytics Teams data wharehouse and ETL platform\) that:

1. Pulls data from a google sheet the Budget Office updates
2. Drops the data into Civis' Postgres database
3. Uploads the tables as csv to s3, replacing existing files of the same name with new information.

With this workflow, we can ensure that when the Budget Office needs to make a change on a chart on boston.gov, they only have to update the agreed upon spreadsheet. More detailed documentation on this process.\(**add link**\)

### Making a chart - examples

## Technical Docs

### Web Component

### Drupal integration

