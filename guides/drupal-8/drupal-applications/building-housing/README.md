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

## Content Types

### Primary Content Types

* BH Project

#### Support Content Types

* BH Meeting
* BH Parcel
* BH Parecl-Project Association
* BH Update

#### Taxonomies

* Disposition Type (BH)
* Funding Stage (BH)
* Neighborhood (BH)
* Project Stage (BH)
* Project Status (BH)
* Project Type (BH)
* Project Update Type (BH)
* Property Type (BH)
* Public Stage (BH)
* Record Type (BH)

***

## Search for Projects on the Building Housing Map

From the building housing landing page a user can click and open a map showing all the current projects. The map and sidebar list are both generated via Building Housing View.

**Entry point**: /buildinghousing (click show map)

**Custom CSS**:

* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/css/node\_bh\_landing\_page.css
* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/css/views\_bh\_listings.css

**Views Template**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/templates/views-view--bhmaps--maplist.html.twig

**Views Functions**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/node\_buildinghousing.views.inc

**Custom Markers**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/images

***

## View details of a Project

This feature shows details about a Building Housing Project. Any new data is pulled from salesforce on every CRON run. Display for the building housing project page is mainly controlled by the BH Project display.

**Entry point**: /buildinghousing (click show map)

**URL pattern**: /buildinghousing/{ProjectName}

**Custom CSS**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/css/node\_bh\_project.css

**Field Templates**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/templates/snippets

**Field Formatters**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/Plugin/Field/FieldFormatter

**Helper Functions** (Pre-process, alters, fields):

* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/node\_buildinghousing.module
* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/BuildingHousingUtils.php

#### Components & Fields (See Image)

1. **Parcel Map** - settings
   * Uses BuildingHousingUtils->setParcelGeoPolyData() and Arcgis to set the Polygon geo data for the parcels
2. **Photo gallery** - settings
3. **Building Housing Project Map** (same as above Map feature)
4. **Project information**
5. **Developer Information** - Custom field in node\_buildinghousing.module
6. **Project Goals**
7. **Project Timeline** - Altered field to combine other fields
   * docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/Plugin/Field/FieldFormatter/EntityReferenceTaxonomyTermBSPublicStageFormatter.php
   * docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/BuildingHousingUtils.php
   * [More Information](project-timeline.md)
8. **Project Type** - Custom field in node\_buildinghousing.module
9. **Contact information** - Custom field in node\_buildinghousing.module
10. **Email sign-up** - Custom field in node\_buildinghousing.module
11. **Feedback form** - settings

![](../../../../.gitbook/assets/BuildingHousing\_ProjectPage.png)

## Auto-create Community Meeting Events from Sales Force on CRON (5 minutes)

A DND Development Officer is able to create a Meeting object in Sales Force, with all the meeting information, and attach it to a Project in Sales Force. When CRON runs on Drupal it then will sync any new or updated Meetings from Sales Force with a Drupal BH Meeting. After the new meeting is crated in Drupal, we also creat a Drupal Event so that the meeting will be listed on the Boston.gov Events page. The Meeting is also then displayed on the corresponding Drupal BH Project.

**BH Meeting Content Type**: /admin/structure/types/manage/bh\_meeting

**Sales Force Mappings**: /admin/structure/salesforce/mappings/manage/bh\_community\_meeting\_event

**Templates**:

* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/templates/snippets/bh-project-meeting-notice.html.twig
* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/templates/snippets/bh-project-timeline-meeting.html.twig

**Helper Functions** (Pre-process, alters):

* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/node\_buildinghousing.module
* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/BuildingHousingUtils.php

***

## Auto-create and update Projects from Sales Force on CRON (5 minutes)

This feature allows Drupal entities to sync back and forth with Sales Force Objects via the Drupal Sales Force module. It is primarily used by DND to use the data and access that is already on DND's Sales Force server to automatically sync with the Boston.gov Drupal site. This is controlled by field mapping configurations in the Drupal Sales Force module. Currently, all syncing is scheduled to happen on Drupal CRON run, every 5 minutes, with only updated objects.

**Sales Force Mappings**:

* Building Housing - Projects (/admin/structure/salesforce/mappings/manage/building\_housing\_projects/fields)
  * bh\_project --> Project\_\_c
* Building Housing - Website Update (/admin/structure/salesforce/mappings/manage/bh\_website\_update/fields)
  * bh\_update --> Website\_Update\_\_c
* Building Housing - Project Update (/admin/structure/salesforce/mappings/manage/building\_housing\_project\_update/fields)
  * bh\_update --> Update\_\_c
* BH Community Meeting Event (/admin/structure/salesforce/mappings/manage/bh\_community\_meeting\_event/fields)
  * bh\_meeting --> Community\_Meeting\_Event\_\_c
* Building Housing - Parcels (/admin/structure/salesforce/mappings/manage/building\_housing\_parcels/fields)
  * bh\_parcel --> Parcel\_\_c
* Building Housing - Parcels-Project Assoc (/admin/structure/salesforce/mappings/manage/bh\_parcel\_project\_assoc/fields)
  * bh\_parcel\_project\_assoc --> ParcelProject\_Association\_\_c

**Sales Force Settings**:

* Building Housing - Projects (/admin/structure/salesforce/mappings/manage/building\_housing\_projects)
* Building Housing - Website Update (/admin/structure/salesforce/mappings/manage/bh\_website\_update)
* Building Housing - Project Update (/admin/structure/salesforce/mappings/manage/building\_housing\_project\_update)
* BH Community Meeting Event (/admin/structure/salesforce/mappings/manage/bh\_community\_meeting\_event)
* Building Housing - Parcels (/admin/structure/salesforce/mappings/manage/building\_housing\_parcels)
* Building Housing - Parcels-Project Assoc (/admin/structure/salesforce/mappings/manage/bh\_parcel\_project\_assoc)

> **Troubleshoot Sales Force connection issues**
>
> If Drupal and Sales Force are not connecting or syncing please check the Authorization from Drupal to Sales Force (/admin/config/salesforce/authorize/list). You may need to Re-auth or even make a new connection if you need to connect to a lower development or testing environment on Sales Force. If you need access to an instance contact DND's Sales Force developer/administrator.
>
> If a single item is not syncing or if you need info about the Drupal to Sales Force connection you can view the list this admin page. If you edit the instance you then have the option to force pull or push the Drupal entity with the Sales Force Object. If there is an issue you should see an error message in the response. You can also find other useful info like timestamps and record ids.
