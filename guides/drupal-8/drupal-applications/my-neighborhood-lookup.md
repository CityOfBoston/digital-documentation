---
description: >-
  A drupal module that displays various pieces of information about the entered
  address.
---

# My Neighborhood Lookup

## Drupal Component&#x20;

The my neighborhood application is a Drupal component that can be added to any page on boston.gov. Currently, it is here: [https://www.boston.gov/my-neighborhood](https://www.boston.gov/my-neighborhood)&#x20;

### Drupal Import Scripts

There are two Drupal endpoints associated with this application.one for receiving the updates

* One for receiving the updated records on a nightly basis (updates): [`https://www.boston.gov/rest/mnl/update`](https://www.boston.gov/rest/mnl/update?)``
* One for receiving the full load of records: [`https://www.boston.gov/rest/mnl/import`](https://www.boston.gov/rest/mnl/import)``

[This page](https://content.boston.gov/admin/config/services/mnl) has information on the status of Drupal import scripts that run nightly and once a month.&#x20;

## Data Sources

The data used in this application come from a variety of GIS data sources. [This spreadsheet](https://docs.google.com/spreadsheets/d/1TsVNjMCO8yMbAEWCAfy24xx8HnTNvL5cJ58sg1nQAtQ/edit?usp=sharing) lists them all in addition to the workflow that brings each one into Civis.&#x20;

These datasources are combined with the SAM address dataset in Civis. [This workflow](https://github.com/CityOfBoston/civis\_pipelines/blob/dcfe088e154f010e9090ee3d57130882d7dbdaaa/workflows/my\_neighborhood.yaml) is the one that combines all the datasets.&#x20;

Every night, [the my neighborhood workflow](https://github.com/CityOfBoston/civis\_pipelines/blob/dcfe088e154f010e9090ee3d57130882d7dbdaaa/workflows/my\_neighborhood.yaml) runs and sends the any records that have updated or changed to boston.gov. Once a month on the 1st, the workflow sends the entire load of records to Drupal.

**NOTES:** There are two cards that show data which are hard coded and don't sync with Civis. They are the mayor's name in the "YOUR MAYOR" card found in "mnl\_config.js" file and the "YOUR AT-LARGE CITY COUNCILORS" card found in "Representation.js" file.&#x20;

In addition to the hard coded items, there too is a data dependency on ReCollect. We built an endpoint in Drupal "rest/recollect" found in Drupal module "bos\_mnl" to query ReCollect API with the user's inputted address and returns the next trash and recycling date for that specific address.

Jonathan Porter (Analytics division of DoIT) is the best point of contact for the Civis portion. Matt McGowan (Digital division of DoIT) is the best point of contact for the Drupal REST information.

**Product**

* ****[Product requirements](https://docs.google.com/document/d/1w\_BknpFRHAudTEO2wdSdPIFboNSroY5LHmMWn4vwjRA/edit#heading=h.kfsq8a92ggxf) (historical and current)\


Historical work that \*potentially\* isn't reflected above: [https://drive.google.com/drive/u/0/folders/1QVSDn6CgJiMY7dbYEiKKDRRX1cYOEHmu](https://drive.google.com/drive/u/0/folders/1QVSDn6CgJiMY7dbYEiKKDRRX1cYOEHmu)



