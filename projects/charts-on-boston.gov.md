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

The general overview for getting a chart on boston.gov is:

1. Get some data
2. Work with the Analytics Team to get it into a public s3 bucket and set up an automated workflow for it getting updated \(if appropriate\).
3. Use the [Online Vega Editor](https://vega.github.io/editor/#/edited) to start building your chart with your s3 url for the data. 
4. Check that your chart works/looks okay using the [COB-chart editor](https://patterns.boston.gov/web-components/chart-editor.html).
5. Add your chart to boston.gov through Drupal. 

### Getting the data with the Analytics Team

Vega/VegaLite can read in data from a publicly available url. We leverage this functionality so that we can set up separate automation practices for the data charts on boston.gov display. This helps ensure the data on the chart stays up-to-date.

For testing purposes, you could upload a csv as a [Github Gist](https://gist.github.com/) and use that url, but when things move to production, all data should be pushing/pulling from an s3 bucket.

#### Using s3

The Analytics Team has the ability to create s3 buckets that can store completely public csv files. Once a file has been loaded to a public bucket, you can supply the "Object URL" to Vega. 

![Location of the &quot;Object URL&quot; in s3. ](../.gitbook/assets/image%20%285%29.png)

Storing data for charts on s3, means we can set up separate workflows that will automatically update the data on the chart. For example, for the [FY20 Budget website](https://www.boston.gov/departments/budget/fy20-budget), we set up a workflow in [Civis](https://app.gitbook.com/@boston/s/analytics/guides/civis/introduction) \(the Analytics Teams data wharehouse and ETL platform\) that:

1. Pulls data from a [publicly available google sheet](https://docs.google.com/spreadsheets/d/1sH5MLdzepDDeEFRksFjp9hVJ3beU87Q5nIJCEr9kIy0/edit?usp=sharing) the Budget Office updates
2. Drops the data into Civis' Postgres database
3. Uploads the tables as csv to s3 \(e.g. [https://s3.amazonaws.com/public-budget-data/fy20\_capital\_projects\_chart\_data.csv](https://s3.amazonaws.com/public-budget-data/fy20_capital_projects_chart_data.csv)\), replacing existing files of the same name with new information. 

With this workflow, we can ensure that when the Budget Office needs to make a change on a chart on boston.gov, they only have to update the agreed upon spreadsheet. More detailed documentation on this process.\(**add link**\)

## Making a chart

To create the charts, we use a json schema created an maintained by the authors of Vega/VegaLite. We primarily rely on VegaLite as it is easier to use than Vega. **The only time Vega is used is to create a pie chart.** 

A good place to get started for an overall understanding of the libraries is their docs: 

* [VegaLite schema documentation](https://vega.github.io/vega-lite/docs/)
* [Vega schema documentation](https://vega.github.io/vega/docs/)

The Vega libraries can do much more than what we've implemented at the City \(e.g. create [maps](https://vega.github.io/vega-lite/examples/geo_choropleth.html), [box plots](https://vega.github.io/vega-lite/examples/boxplot_2D_vertical.html), [multi-view displays](https://vega.github.io/vega-lite/examples/trellis_area.html), [treemaps](https://vega.github.io/vega/examples/treemap/), [etc](https://vega.github.io/vega-lite/examples/).\). The charts web component was **built and tested for only the following** charts:

| Chart Type | Vega or VegaLite |
| :--- | :--- |
| Bar Charts | VegaLite |
| Line Charts | VegaLite |
| Grouped Bar Charts | VegaLite |
| Pie Charts | Vega |
| Area Charts | VegaLite |

Each of the above charts can be built with **one selection element** as well. 

When developing a chart, it will likely be easiest to start using the [Vega/VegaLite Online editor](https://vega.github.io/editor/#/edited). Once the chart has largely been built, the [COB-chart editor](https://patterns.boston.gov/web-components/chart-editor.html) is helpful to make sure the chart will still work when rendered as a web component and for making sure fonts look okay. 

#### The "boston" section of the schema

The `<cob-chart>` web component supports an additional section of the schema called "boston". This section supports three inputs specific to our uses cases: 

* "chartID" - _string, optional:_ unique id of the chart
* "minWidth"- _integer, optional:_ minimum width in pixels of the chart. Due to data constraints, some charts will not be readable on smaller screens. In this case, we can supply a minimum width to the chart and users will be able to horizontal scroll on any screen smaller than the minimum width supplied. [Example of this functionality in production](https://www.boston.gov/departments/budget/fy20-budget/fy20-capital-budget#spending). 
* "defaultSelection": _string, optional:_ value of a drop down's default selection

Below are examples of how each specific type of chart gets implemented at the City of Boston. Links to example schemas are given with call outs of particularly interesting functionality the chart contains. 

#### Height and Width

The **height** of the chart defined in the schema will be respected and used when the chart is rendered as a web component on a boston.gov page. 

The **width** of the chart will be overwritten based on the screen size the chart is rendered on if `"autosize": "fit"` is in the schema. This helps ensure a chart fits on both large and small screens. 

```text
  {
    ...,
    "height": 450,
    "width": 400,
    "autosize": "fit",
    ...
  }
```

### Bar Charts

{% tabs %}
{% tab title="Bar Chart" %}
Drop the JSON from the file linked below in the [COB chart editor](https://patterns.boston.gov/web-components/chart-editor.html) to see the chart below \([chart in production](https://www.boston.gov/departments/budget/fy20-education-overview#universal-pre-k)\):

![&amp;lt;cob-chart&amp;gt; simple bar chart.](../.gitbook/assets/image%20%283%29.png)

{% file src="../.gitbook/assets/bar-chart-schema.json" caption="Bar Chart schema JSON" %}

#### Commented Bar Chart schema

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

{% tab title="Bar Chart with Selection" %}
Drop the json linked below into the [COB chart editor](https://patterns.boston.gov/web-components/chart-editor.html) to see the chart below \([chart in production](https://www.boston.gov/departments/budget/fy20-budget/fy20-capital-budget)\):

![&amp;lt;cob-chart&amp;gt; bar chart with selection.](../.gitbook/assets/image%20%286%29.png)

{% file src="../.gitbook/assets/bar-chart-select-schema.json" caption="Bar Chart with Selection schema JSON" %}

#### Commented chart schema

```text
{
-- $schema tells the component whether or not we are using Vega or VegaLite.
    "$schema": "https://vega.github.io/schema/vega-lite/v2.json",
    -- boston section of the schema outlined above.    
    "boston": {
        "chartID": "barChart",
        -- This chart won't autosize to be smaller than 600px.
        "minWidth": 600,
        -- Default selected item when the chart loads.
        "defaultSelection": "Environment Department"
    },
    "height": 400,
    -- Will get overwritten if screen is larger than 600px because 
    -- of "autosize": "fit" being set but having "minWidth":600 in the 
    -- "boston" section of the schema.
    "width": 600,
    "autosize": "fit",
    -- Defining information about the select element.
    "selection": {
        "select": {
            "type": "single",
            -- <cob-chart> only supports one field for selecting from.
            "fields": [
                "Department_and_Category"
            ],
            "bind": {
            -- bind this to a select element specifically.
                "input": "select",
                "element": "#select",
                -- Instructions above element.
                "name": "Select a Department: "
            }
        }
    },
    "data": {
        "name": "capitalProjects",
        "url": "https://s3.amazonaws.com/public-budget-data/fy20_capital_projects_chart_data.csv",
        -- Ensure specific fields are read in as numbers.
        "format": {
            "parse": {
                "Total_Project_Budget": "number",
                "Expended": "number"
            }
        }
    },
    -- Change the shape of the table from long to wide to help
    -- charting. 
    "transform": [
        {
            "fold": [
                "Total_Project_Budget",
                "Expended"
            ],
            "as": [
                "Type",
                "Amount"
            ]
        },
        -- Calculate fields for displaying. The data comes with underscores in Type field values. We calculate a new
        -- field without those so the legend values show nicely. 
        {
            "calculate": "if(datum.Type === 'Total_Project_Budget', 'Total Project Budget', 'Expended through FY2018')",
            "as": "TypeFormatted"
        },
        -- Need this to ensure that selecting a dropdown element will update 
        -- the chart.
        {
            "filter": {
                "selection": "select"
            }
        }
    ],
    "mark": "bar",
    "encoding": {
    -- Color the chart by Type. We use the calculated "TypeFormatted" field
    -- so that the legend uses the values from this field that are easier
    -- to read. 
        "color": {
            "field": "TypeFormatted",
            "type": "nominal",
            "scale": {
            -- Colors for the Type values.
                "range": [
                    "#66A5D9",
                    "#1871bd"
                ]
            },
            "legend": {
            -- We leverage fonts from COB brand.
                "orient": "bottom",
                "labelFont": "Montserrat",
                "labelLimit": 500,
                "labelFontSize": 16,
                -- Distance from chart in pixels.
                "offset": 10,
                -- We don't want a legend title since it would be the field name.
                "title": "",
                -- Make sure the legend shows vertically instead of horizontally.
                "direction": "vertical"
            }
        },
        "y": {
            "field": "Project_Name",
            "type": "nominal",
            "axis": {
                "title": ""
            },
            -- We sort bars on the y-axis so that the highest values are at
            -- the top. 
            "sort": {
                "op": "sum",
                "field": "Total_Project_Budget",
                "order": "descending"
            }
        },
        "x": {
            "field": "Amount",
            "type": "quantitative",
            "axis": {
                "title": "",
                "grid": true
            },
            "stack": null
        },
        "tooltip": [
            {
                "field": "Total_Project_Budget",
                "type": "quantitative",
                "format": "$,r",
                -- Title is what will display in the tooltip.
                "title": "Project Budget"
            },
            {
                "field": "Expended",
                "type": "quantitative",
                "format": "$,"
            },
            {
                "field": "Department",
                "type": "nominal",
                "title": "Department"
            },
            {
                "field": "Status",
                "type": "nominal",
                "title": "Status"
            }
        ]
    },
    "config": {
        "numberFormat": "s",
        "axis": {
        -- Use fonts to match the COB brand. 
            "labelFont": "Lora",
            "labelFontSize": 16,
            "labelLimit": 500,
            "titleFont": "Montserrat",
            "titleFontSize": 16,
            "titleFontWeight": "normal",
            "titlePadding": 15
        }
    }
}
```
{% endtab %}
{% endtabs %}

### Helpful Hints

#### Ordering the Colors on a Bar Chart

If you want the colored sections of bar charts to display in a specified order, you can use the VegaLite [transform](https://vega.github.io/vega-lite/docs/transform.html) to create a new field in the data. In the example below, this field is called `"barOrder"`.

```text
"transform": [
    {
      "calculate": "if(datum.Category == 'Community-Based Organizations', 2, 1)",
      "as": "barOrder"
    }
  ],
```

You can then use that field to [order the bars](https://vega.github.io/vega-lite/docs/stack.html#order) so that the number of Community-Based Organization seats is always on top.

```text
  "order": {
    "field": "barOrder",
    "type": "quantitative"
  },
```

#### Sort bars on Bar Chart axis

You can use the ["sort" section](https://vega.github.io/vega-lite/docs/sort.html) of the schema or an axis to sort by various metrics. For example, we may want to have the longest bars at the top of a chart on a bar chart. 

![Bars sorted by &quot;amount&quot;.](../.gitbook/assets/image%20%284%29.png)

To achieve that, we add `"sort"` and define the field, operation, and order of the bars to the axis definition we want sorted.

```text
        "y": {
            "field": "Project_Name",
            "type": "nominal",
            "axis": {
                "title": ""
            },
            -- We sort bars on the y-axis so that the highest values are at
            -- the top. 
            "sort": {
                "op": "sum",
                "field": "Total_Project_Budget",
                "order": "descending"
            }
        },
```

#### Using Domain and Range together

To ensure each value is always the same color - just ensure ppl cant/won't edit the values.

#### Ensure specific fields are read in as numbers

format parse

#### Folding table

#### Selections

only one field supported

always a select element

### Known limitations



## Technical Docs

### Web Component

### Drupal integration

