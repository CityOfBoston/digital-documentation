---
description: >-
  This page documents data sources and pipelines for maps on boston.gov that use
  the maps Drupal component.
---

# Maps on boston.gov

## Maps in production

<table>
  <thead>
    <tr>
      <th style="text-align:left">Page with URL</th>
      <th style="text-align:left">Source Dataset(s)</th>
      <th style="text-align:left">ArcGIS Online Dataset(s)</th>
      <th style="text-align:left">Pipeline Documentation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://www.boston.gov/departments/small-business-development/city-boston-food-trucks-schedule#food-trucks-map">Food Trucks Map</a>
      </td>
      <td style="text-align:left"><a href="https://docs.google.com/spreadsheets/d/1uQYMXIBaOzbTrm9jwgoDD4xkBkQG8dNdEhBj9Mq_K6I/edit?usp=sharing">Google Sheet</a>
      </td>
      <td style="text-align:left"><a href="http://boston.maps.arcgis.com/home/item.html?id=437d8cb34b58481b9d7b07ea88912b0c">food_truck_schedule</a>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>2019 Marathon Map</p>
        <ul>
          <li><a href="https://www.boston.gov/news/patriots-day-weekend-2019#map">Patriots Day Weekend</a>
          </li>
          <li><a href="https://www.boston.gov/news/2019-boston-marathon-traffic-advisory#map">Traffic Advisory</a>
          </li>
        </ul>
      </td>
      <td style="text-align:left">Hosted feature services created by OEM.</td>
      <td style="text-align:left">
        <p><a href="https://services.arcgis.com/sFnw0xNflSi8J0uh/ArcGIS/rest/services/Boston_Marathon_2019_Road_Closures/FeatureServer/">2019 Road closures </a>
        </p>
        <p><a href="https://services.arcgis.com/DO4gTjwJVIJ7O9Ca/arcgis/rest/services/Boston_Marathon_Route/FeatureServer">Route</a>
        </p>
        <p><a href="https://services.arcgis.com/sFnw0xNflSi8J0uh/arcgis/rest/services/ParkingRestrictions2019/FeatureServer">2019 Parking restrictions</a>
        </p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>## Creating a map

TODO: Drupal instructions

For info about the `<cob-map>` web component and its attributes, see the “Notes” pane in the [`<cob-map>` Fleet documentation](https://patterns.boston.gov/components/detail/map--default.html).

The JSON configuration for the web component is specified in [map-1.0.json.schema](https://github.com/CityOfBoston/patterns/blob/develop/web-components/map/map-1.0.schema.json) in the CityOfBoston/patterns repo. Auto-generated documentation for it can be found here: [map-1.0 schema documentation](https://patterns.boston.gov/vendor/docson/#/web-components/map-1.0.schema.json).

## Technical Documentation

The embedded maps feature was written as a web component so that it could be added to the Boston.gov Drupal site but also any other web pages we make. It is built using the [StencilJS](https://stenciljs.com/) reactive web component compiler.

Source code for `<cob-map>` and our other web components is in the Fleet repo: [CityOfBoston/patterns](https://github.com/CityOfBoston/patterns)



