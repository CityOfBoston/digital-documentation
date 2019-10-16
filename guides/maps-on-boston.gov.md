---
description: >-
  This page documents data sources and pipelines for maps on boston.gov that use
  the maps Drupal component.
---

# Maps on boston.gov

## Maps that have been created

This [spreadsheet](https://docs.google.com/spreadsheets/d/1qBQrTCPEveSf-M6sPJ1HVInlijNmvXrf156Wc5o38UY/edit?usp=sharing) contains all maps in that have been created.

## Creating a map for boston.gov

Below are the main steps in getting a map on boston.gov:

1. Get data
2. Make sure the data stays up-to-date
3. Create a map on boston.gov in Drupal

### Get Data

### Make sure the data stays up to date

### Create the map on boston.gov

## Technical Documentation

The embedded maps feature was written as a web component so that it could be added to the Boston.gov Drupal site but also any other web pages we make. It is built using the [StencilJS](https://stenciljs.com/) reactive web component compiler.

For info about the `<cob-map>` web component and its attributes, see the “Notes” pane in the [`<cob-map>` Fleet documentation](https://patterns.boston.gov/components/detail/map--default.html).

The JSON configuration for the web component is specified in [map-1.0.json.schema](https://github.com/CityOfBoston/patterns/blob/develop/web-components/map/map-1.0.schema.json) in the CityOfBoston/patterns repo. Auto-generated documentation for it can be found here: [map-1.0 schema documentation](https://patterns.boston.gov/vendor/docson/#/web-components/map-1.0.schema.json).

Source code for `<cob-map>` and our other web components is in the Fleet repo: [CityOfBoston/patterns](https://github.com/CityOfBoston/patterns)

