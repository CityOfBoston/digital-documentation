---
description: >-
  Building Housing allows constituents to see a full inventory of projects and
  parcels managed by the city of Boston.
---

# Building Housing

## Description

This Drupal App allows residents to browse all active housing, open space, commercial, and to be decided (TBD) projects. It also provides information on city-owned land for sale.

Residents can click search for a specific project and/or view a map of all projects. A drill-down into a project page displays goals, a timeline, photos, meetings and more.&#x20;

## Features

* Search for Projects on the Building Housing Map
* View details of a Project
* Auto-create Community Meeting Events from Sales Force on CRON (5 minutes)
* Auto-create and update Projects from Sales Force on CRON (5 minutes)

## Technologies Used

* Drupal
* Sales Force
* Google Maps API

## Major Drupal Modules used

* node\_buildinghousing (docroot/modules/custom/bos\_content/modules/node\_buildinghousing)
* Views
* Webforms
* Geolocation
* Salesforce

{% content-ref url="bh-drupal-entities.md" %}
[bh-drupal-entities.md](bh-drupal-entities.md)
{% endcontent-ref %}

{% content-ref url="bh-map-webpage.md" %}
[bh-map-webpage.md](bh-map-webpage.md)
{% endcontent-ref %}

{% content-ref url="bh-property-webpage/" %}
[bh-property-webpage](bh-property-webpage/)
{% endcontent-ref %}

{% content-ref url="bh-salesforce-sync/" %}
[bh-salesforce-sync](bh-salesforce-sync/)
{% endcontent-ref %}

