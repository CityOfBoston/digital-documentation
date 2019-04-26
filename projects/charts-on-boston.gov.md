---
description: Technical and project documentation for charts on boston.gov.
---

# Charts on boston.gov

We use the [VegaLite](https://vega.github.io/vega-lite/) and [Vega](https://vega.github.io/vega/) libraries to create our charts on boston.gov. These are flexible libraries that let users define/create a chart using a JSON schema. 

We wrap this library up as a web component that is stored in Fleet, our patterns library. The `<cob-chart>` component takes a JSON object as input in a Vega or VegaLite schema as input and creates a chart. 

## Putting a chart on boston.gov

In general, content on boston.gov should be at or below an 8th grade reading level. In an attempt to be in accordance with this and keep our site as easy-to-understand as possible, we push for simple and straight forward charts \(e.g. bar charts, line charts, and sometimes pie charts if we have to\). 

### Getting the data

Vega/VegaLite can read in data from a publicly available url. We leverage this functionality so that we can set up separate automation practices for the data charts on boston.gov display. 

#### Using s3

The Analytics Team has the ability to create s3 buckets that can store completely public csv files. **Link to budget pipeline docs.** Once a file has been loaded to a public bucket, you can supply the "Object URL" to Vega. 

### Making a chart - examples

## Technical Docs

### Web Component

### Drupal integration

