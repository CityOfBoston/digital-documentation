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

## Get Data

There are two main ways you can set up the data for maps on boston.gov: 

1. Google Sheet
2. ArcGIS Online hosted feature service 

### Google Sheet Data Source

If the stakeholder you are working with is more comfortable editing and updating information in a Google Sheet, you can create a hosted feature service using this. 

The Google Sheet you set up needs to have one of two things:

1. Latitude and Longitude fields \(preferred method\)
2. An address field that contains the locations entire address \(e.g. 200 Heath St, Jamaica Plain, MA 02130\)

Using latitude and longitude fields allows the connection between the Google Sheet and the created ArcGIS hosted feature service to be **automated**. If you use the address field, the Google Sheet and the feature service will not be connected and you'll have to manually update it every time it changes.

To create a new feature service using Google Sheets, you need to:

 1. Download it as a csv file:

![Download a google sheet as a csv file.](../.gitbook/assets/screen-shot-2020-10-01-at-10.50.26-am.png)

2. Re-name the file to whatever it is you want your feature service to  be named.

3. Log into [BostonMaps](http://boston.maps.arcgis.com/home/index.html) under the ETL developers username.

4. Create a new folder for this feature service.

![Create a new folder under the &quot;Content&quot; page in the ETL Developers BostonMaps log in.](../.gitbook/assets/screen-shot-2020-10-01-at-10.54.58-am.png)

5. Click on "Add Item" the "From Computer":

![Add an item to BostonMaps from your computer.](../.gitbook/assets/screen-shot-2020-10-01-at-10.56.13-am.png)

6. Choose the file you want to upload. Give it clear tags and confirm that it is using the correct fields for locations. If you are using addresses instead of Latitude and Longitude fields, make sure "ArcGIS World Geocoding Service" is selected.

Below is a screen recording of this process:

{% file src="../.gitbook/assets/adding-csv-file-to-bostonmaps.mov" caption="Add csv file to BostonMaps" %}

7. Once the feature service is created, you need to make sure the share settings are set to "public". If you don't do this, we won't be able to access the service from boston.gov.

![Update the created feature service to be set to &quot;public&quot;.](../.gitbook/assets/share-settings-set-to-public.png)

### Use an ArcGIS hosted feature service.

If the stakeholder you are working with is comfortable editing hosted feature services in ArcGIS Online, you can use that as well. **The only thing needed for the boston.gov maps is that the feature layer be shared publicly**. 

It is not ideal to have publicly accessible hosted feature services, therefore it is best practice to **create a View of the feature service you are using.** You can then set that view to be publicly accessible and use that for the map while the editable layer stays internal. 

An example of this set up is with the COVID-19 testing locations:

* [Public view layer](http://boston.maps.arcgis.com/home/item.html?id=a1dc6adebe7545e2ba22e45ab919a3e4)
* [Underlying internal layer edited by BPHC](http://boston.maps.arcgis.com/home/item.html?id=a1fab075b0dd46329392de6d0e19fb48)

You can create an ArcGIS web application that allows the stakeholder to edit and update the data if you want. Or, if they are comfortable just using the "Open in map viewer with full editing control", you can have them use that. 

The pro of setting the map up this way is that when the stakeholder makes an update it is _immediately and automatically_ updated on the boston.gov map. 

## Make sure the data stays up to date

### Editing a ArcGIS Hosted Feature Service Directly

If you are using an ArcGIS hosted feature service and having the stakeholder edit that, you just need to get them directions on how to add to a map with full editing control, they can then update the service. 

### Google Sheet with Latitude and Longitude fields

If you are using a google sheet that has Lat/Long fields, you need to give the stakeholder instructions on how to update that information. Generally, we point people to google maps. [Here is a screen recording of how to get the values](https://drive.google.com/file/d/1tgKH77jO7h2YVf1gXb4D9Dvuti5NyeAW/view?usp=sharing).

Once you have that, you can submit a ticket to the Data Engineering team via [this form](https://app.smartsheet.com/b/form/eb1e9526243b4278bd73ae4b98ef9018). Be sure to include the link to the Google Sheet, the link to the hosted feature service in your request, and the frequency with which you want the pipeline to run \(most if not all are nightly\).

It might take a few days for the connection to get automated, and you may have a time sensitive map you are working on \(e.g. early voting locations, covid testing sites, etc.\). In this case, you can follow the instructions for manually updating a map below.

### Google Sheet without Lat/Long fields or that is just not automated yet

To manually update a map:

1. Download the spreadsheet as a csv file.
2. Rename it to match the hosted feature service.
3. Navigate to the hosted feature service's "Overview" page in ArcGIS Online.
4. Click on "Update Data" then "Overwrite Entire Layer"
5. Chose the csv file you downloaded are renamed.

{% file src="../.gitbook/assets/overwrite-entire-layer.mov" caption="Manually update hosted feature service" %}

## Things to note

Dates can be weird; addresses can get geocoded to odd locations





## Create the map on boston.gov

## Technical Documentation

The embedded maps feature was written as a web component so that it could be added to the Boston.gov Drupal site but also any other web pages we make. It is built using the [StencilJS](https://stenciljs.com/) reactive web component compiler.

For info about the `<cob-map>` web component and its attributes, see the “Notes” pane in the [`<cob-map>` Fleet documentation](https://patterns.boston.gov/components/detail/map--default.html).

The JSON configuration for the web component is specified in [map-1.0.json.schema](https://github.com/CityOfBoston/patterns/blob/develop/web-components/map/map-1.0.schema.json) in the CityOfBoston/patterns repo. Auto-generated documentation for it can be found here: [map-1.0 schema documentation](https://patterns.boston.gov/vendor/docson/#/web-components/map-1.0.schema.json).

Source code for `<cob-map>` and our other web components is in the Fleet repo: [CityOfBoston/patterns](https://github.com/CityOfBoston/patterns)

