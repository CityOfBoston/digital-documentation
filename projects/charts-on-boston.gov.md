---
description: Technical and project documentation for charts on boston.gov.
---

# Charts on boston.gov

We use the [VegaLite](https://vega.github.io/vega-lite/) and [Vega](https://vega.github.io/vega/) libraries to create our charts on boston.gov. VegaLite is an easier to use version of Vega. It does a little more guess work for us so we don't have to be so specific in defining our charts. These libraries let users define/create a chart using a JSON schema. 

From a technical standpoint, we wrap this library up as a web component that is stored in [Fleet](fleet-pattern-library.md), our patterns library. The `<cob-chart>` component takes a JSON object as input in a Vega or VegaLite schema as input and creates a chart. 

From a functional standpoint, when putting charts on boston.gov, we place them within text components in an effort to bring context to the data and information the chart is displaying. The [FY20 Budget Website](https://www.boston.gov/departments/budget/fy20-budget) has good examples of how charts should be used with narrative explanation. 

#### Good Reads

* [How Vega works Observable Notebook](https://observablehq.com/@vega/how-vega-works)
* [Guide to Axes & Legends in Vega](https://observablehq.com/@vega/a-guide-to-guides-axes-legends-in-vega)

## Putting a chart on boston.gov

In general, content on boston.gov should be at or below an 8th grade reading level. In an attempt to be in accordance with this and keep our site as easy-to-understand as possible, we push for simple and straight forward charts \(e.g. bar charts, line charts, and sometimes pie charts if we have to\). 

Coordination with both the boston.gov Content Manager \(for chart placement\) and the Analytics Team \(for chart data\) will be required to get a chart on boston.gov. 

### Getting the data with the Analytics Team

Vega/VegaLite can read in data from a publicly available url. We leverage this functionality so that we can set up separate automation practices for the data charts on boston.gov display. This helps ensure the data on the chart stays up-to-date.

For testing purposes, you could upload a csv as a [Github Gist](https://gist.github.com/) and use that url, but when things move to production, all data should be pushing/pulling from an s3 bucket.

#### Using s3

The Analytics Team has the ability to create s3 buckets that can store completely public csv files. Once a file has been loaded to a public bucket, you can supply the "Object URL" to Vega. 

![Location of the &quot;Object URL&quot; in s3. ](../.gitbook/assets/image%20%284%29.png)

Storing data for charts on s3, means we can set up separate workflows that will automatically update the data on the chart. For example, for the [FY20 Budget website](https://www.boston.gov/departments/budget/fy20-budget), we set up a workflow in [Civis](https://app.gitbook.com/@boston/s/analytics/guides/civis/introduction) \(the Analytics Teams data wharehouse and ETL platform\) that:

1. Pulls data from a [publicly available google sheet](https://docs.google.com/spreadsheets/d/1sH5MLdzepDDeEFRksFjp9hVJ3beU87Q5nIJCEr9kIy0/edit?usp=sharing) the Budget Office updates
2. Drops the data into Civis' Postgres database
3. Uploads the tables as csv to s3 \(e.g. [https://s3.amazonaws.com/public-budget-data/fy20\_capital\_projects\_chart\_data.csv](https://s3.amazonaws.com/public-budget-data/fy20_capital_projects_chart_data.csv)\), replacing existing files of the same name with new information. 

With this workflow, we can ensure that when the Budget Office needs to make a change on a chart on boston.gov, they only have to update the agreed upon spreadsheet. More detailed documentation on this process.\(**add link**\)

### Making a chart

To create the charts, we use a json schema created an maintained by the authors of Vega/VegaLite. We primarily rely on VegaLite as it is easier to use than Vega. **The only time Vega is used is to create a pie chart.** 

A good place to get started for an overall understanding of the libraries is their docs: 

* [VegaLite schema documentation](https://vega.github.io/vega-lite/docs/)
* [Vega schema documentation](https://vega.github.io/vega/docs/)

The Vega libraries can do much more than what we've implemented at the City \(e.g. create [maps](https://vega.github.io/vega-lite/examples/geo_choropleth.html), [box plots](https://vega.github.io/vega-lite/examples/boxplot_2D_vertical.html), [multi-view displays](https://vega.github.io/vega-lite/examples/trellis_area.html), [treemaps](https://vega.github.io/vega/examples/treemap/), [etc](https://vega.github.io/vega-lite/examples/).\). The charts web component was built and tested for only the following charts:

* Bar Charts - _VegaLite_
* Line Charts - _VegaLite_
* Grouped Bar Charts - _VegaLite_ 
* Pie Charts - _Vega_
* Area Charts - _VegaLite_

Each of the above charts can be built with **one selection element** as well. 

#### The "boston" section of the schema

The `<cob-chart>` web component supports an additional section of the schema called "boston". This section supports three inputs specific to our uses cases: 

* "chartID" - _string, optional:_ unique id of the chart
* "minWidth"- _integer, optional:_ minimum width in pixels of the chart. Due to data constraints, some charts will not be readable on smaller screens. In this case, we can supply a minimum width to the chart and users will be able to horizontal scroll on any screen smaller than the minimum width supplied. [Example of this functionality in production](https://www.boston.gov/departments/budget/fy20-budget/fy20-capital-budget#spending). 
* "defaultSelection": _string, optional:_ value of a drop down's default selection

Below are examples of how each specific type of chart gets implemented at the City of Boston. Links to example schemas are given with call outs of particularly interesting functionality the chart contains. 

#### Height and Width

The **height** of the chart defined in the schema will be respected and used when the chart is rendered as a web component on a boston.gov page. 

The **width** of the chart will be overwritten based on the screen size the chart is rendered on. 

```text
  {
    ...,
    "height": 450,
    "width": 400,
    "autosize": "fit",
    ...
  }
```

#### Bar Charts

{% tabs %}
{% tab title="Bar Chart" %}
[This schema](https://s3.amazonaws.com/public-budget-data/example-schemas/bar-chart-schema.json) creates the chart below \([chart in production](https://www.boston.gov/departments/budget/fy20-education-overview#universal-pre-k)\):

![&amp;lt;cob-chart&amp;gt; bar chart.](../.gitbook/assets/image%20%283%29.png)

#### Ordering the bars

This chart uses a VegaLite transform to create a new field in the data called `"barOrder"`. 

```text
  "transform": [
    {
      "calculate": "if(datum.Category == 'Community-Based Organizations', 2, 1)",
      "as": "barOrder"
    }
  ],
```

It then uses that field to order the bars so that the number of Community-Based Organization seats is always on top.

```text
    "order": {
      "field": "barOrder",
      "type": "quantitative"
    },
```
{% endtab %}

{% tab title="Annotated Bar Chart Schema" %}
More details about the inputs in this schema can be found in the [VegaLite docs](https://vega.github.io/vega-lite/docs/). 

```text
{
  -- $schema tells the component whether or not we are using Vega or VegaLite.
  "$schema": "https://vega.github.io/schema/vega-lite/v2.json",
  -- boston section of schema explained above.
  "boston": {
  -- Only supply chartID, this chart doesn't have a selection or minWidth.
    "chartID": "upkSeats"
  },
  "height": 450,
  -- This will get overwritten based on the screen size.
  "width": 400,
  -- Ensures the chart will re-size if the screen size changes.
  "autosize": "fit",
  -- URL and name of data source.
  "data": {
    "name": "data",
    "url": "https://s3.amazonaws.com/public-budget-data/fy20_upk_seats.csv"
  },
  "transform": [
  -- Adding a field to use for ordering the bar colors.
    {
      "calculate": "if(datum.Category == 'Community-Based Organizations', 2, 1)",
      "as": "barOrder"
    }
  ],
  "mark": "bar",
  "encoding": {
    "color": {
    -- Defining the field used to color the sections of the bars.
      "field": "Category",
      "type": "nominal",
      -- Defining the colors used in chart.
      "scale": {
        "range": [
          "#1871BD",
          "#66A5D9"
        ]
      },
      "legend": {
        "orient": "bottom",
        -- Using Montserrat to stay within the city's brand.
        "labelFont": "Montserrat",
        "labelLimit": 500,
        "labelFontSize": 16,
        -- Distance in pixels from chart.
        "offset": 10,
        "title": ""
      }
    },
    "x": {
      "field": "Year",
      "type": "ordinal",
      "axis": {
        "title": "",
        "labelAngle": 0
      }
    },
    "y": {
      "aggregate": "sum",
      "field": "Amount",
      "type": "quantitative",
      "axis": {
        "title": "Number of seats",
        "grid": true
      }
    },
    -- Uses the newly calculcated field to order the bar colors on the chart.
    "order": {
      "field": "barOrder",
      "type": "quantitative"
    },
    "tooltip": [
      {
        "field": "Amount",
        "type": "quantitative",
        -- Format numbers in tooltips.
        "format": ",.0f",
        "title": "Seats"
      },
      {
        "field": "Category",
        "type": "nominal",
        "title": "Seat Type"
      },
      {
        "field": "Year",
        "type": "nominal",
        "title": "Year"
      }
    ]
  },
  "config": {
    "numberFormat": "f",
    "axis": {
    -- We define fonts that keep the chart in-line with COB brand.
      "labelFont": "Lora",
      "labelFontSize": 16,
      "labelLimit": 500,
      "titleFont": "Montserrat",
      "titleFontSize": 18,
      "titleFontWeight": "normal",
      "titlePadding": 15
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Technical Docs

### Web Component

### Drupal integration

