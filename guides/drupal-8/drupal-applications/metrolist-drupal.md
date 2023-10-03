# Metrolist (Drupal)

## Description

Metrolist allows Boston residents to search for affordable housing. The Search and AMI Estimator experiences are a JS WebApp. The rest of the app is built in Drupal (this module), with the underlying data layer provided by Salesforce.

Another feature of this module is allowing property managers to list and update their Metrolist listings without directly contacting DND.

## Features

* View details of a MetroList Development listing
* Submit a new Development listing to DND (Sales Force)
* Provide data about Developments via JSON API to the Search and AMI Calculator JS WebApp
* AMI data tables
* Auto-create and update Developments from Sales Force on CRON (5 minutes)

## Technologies Used

* Drupal
* Sales Force
* Google Maps API
* Postmark

## Major Drupal Modules used

* bos\_metrolist (docroot/modules/custom/bos\_content/modules/bos\_metrolist)
* Views
* Webforms
* Geolocation
* Salesforce

## Content Types

### Primary Content Types

* MetroList Development (/admin/structure/types/manage/metrolist\_development)

#### Support Content Types

* MetroList Unit (/admin/structure/types/manage/metrolist\_unit)

#### Taxonomies

* Availability Status
* City
* Due at signing
* Features (MetroList)
* Income Eligibility AMI Threshold
* MetroList: User Guide Type
* Neighborhoods (MetroList)
* Occupancy Type
* Region
* Rent Type
* Unit Type
* Utilities Included



## View details of a MetroList Development listing

This feature shows details about a development and its aggregated units. Any new data is pulled from Sales Force on every CRON run. The display for the development page is mainly controlled by the Available Units View that is included via the Development display.

**Entry point**: /metrolist/search (Use the MetroList Search JS WebApp to find a Development)

**URL pattern**: /metrolist/search/housing/{Development Name}

**Custom CSS**: docroot/modules/custom/bos\_content/modules/bos\_metrolist/css/views\_ml\_block\_avaliable\_units.css

**Views Template**: docroot/modules/custom/bos\_content/modules/bos\_metrolist/templates/views-view-metrolist-drawers.html.twig

**Views Style**: docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/Plugin/views/style/MetroListDrawersStyle.php

**Helper Functions** (Pre-process and alters): docroot/modules/custom/bos\_content/modules/bos\_metrolist/bos\_metrolist.module

#### Components & Fields (See Image)

1. **Location Information**
   * View - Available Units - Header
     * Back to Search
     * Map - settings
     * Development Name
     * Address
     * Area
     * Unit Type
     * Share Listing
2. **Unit Information**
   *   View - Available Units - Fields

       > The Unit entities are aggregated based on type, price, AMI, bedrooms and bathrooms and then related to the proper Development.
3. **Application and lottery information**
   * View - Available Units - Footer
     * Application type and link
     * Agent Contact Information
     * Listing Type Information
     *   Affordable Housing Lean More CTAs

         > A paragraph from the MetroList landing page that is being included from as paragraph library item
     *   Feedback Form

         > A webform used to collect user feedback and send an email to DND contacts



![](../../../.gitbook/assets/MetroList\_DevelopmentsPage.png)

## Submit a new Development listing to DND (Sales Force)

This feature allows project managers throughout the state to submit new developments to DND for review and then eventual listing on MetroList. The user starts by navigating to the MetroList listing form landing page. On this page they then enter their email in the MetroList listing request form. When that webform is submitted a new MetroList Listing form draft is crated and an email is sent to the user with a token link (48 hour ttl). THe user then clicks the privet link and are taken to the Listing form. The listing form then communicates with Sales Force on each step to ether pre-fill information about the user and their properties or create new objects on Sales Force using the custom webform submit handler. Once the form is submitted the new objects will ber created and emails sent to the user and DND.

**Webforms**:

* MetroList Listing Request Form
* MetroList Listing Form

**Webform Handler**: docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/Plugin/WebformHandler/CreateMetroListingWebformHandler.php

**Helper Functions** (Pre-process and alters): docroot/modules/custom/bos\_content/modules/bos\_metrolist/bos\_metrolist.module

**Sales Force Connector** (Uses Sales Force module): docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/MetroListSalesForceConnection.php



## Provide data about Developments via JSON API to the Search and AMI Calculator JS WebApp

This feature used Views REST Export to create a simple JSON API for the MetroList JS WebApp. The application is able to make a request with a number of parameters and receive back MetroList development information and AMI information.

**API Paths**:

* /metrolist/api/v1/developments?\_format=json
* /metrolist/api/v1/ami/hud/base?\_format=json

**Views**:

* /admin/structure/views/view/metrolist/edit/rest\_export\_nested\_1
* /admin/structure/views/view/metrolist\_ami/edit/ml\_ami\_hud\_base

**Views Style**:

* docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/Plugin/views/style/MetroListSerializer.php

**Helper Functions** (Pre-process and alters): docroot/modules/custom/bos\_content/modules/bos\_metrolist/bos\_metrolist.module



## AMI data tables

This feature uses a View to creat a couple of data tables that are then used on income restricted housing guide. It uses the same base data as the AMI API from the Income Eligibility AMI Threshold taxonomy.

**Entry point**: /income-restricted-housing-guide

**Views**:

* /admin/structure/views/view/metrolist\_ami/edit/ami\_table\_hud
* /admin/structure/views/view/metrolist\_ami/edit/ami\_table\_bpda

**Views Field/Validator**:

* docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/Plugin/views/field/AMIThresholdTerms.php
* docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/Plugin/views/argument\_validator/TermAMIThreshold.php

**Views Template**: docroot/modules/custom/bos\_content/modules/bos\_metrolist/templates/views-view-table--metrolist-ami.html.twig

**Helper Functions** (Pre-process and alters): docroot/modules/custom/bos\_content/modules/bos\_metrolist/bos\_metrolist.module



## Auto-create and update Developments from Sales Force on CRON (5 minutes)

This feature allows Drupal entities to sync back and forth with Sales Force Objects via the Drupal Sales Force module. It is primarily used by DND to use the data and access that is already on DND's Sales Force server to automatically sync with the Boston.gov Drupal site. This is controlled by field mapping configurations in the Drupal Sales Force module. Currently, all syncing is scheduled to happen on Drupal CRON run, every 5 minutes, with only updated objects.

**Sales Force Mappings**:

* MetroList - Development (/admin/structure/salesforce/mappings/manage/metrolist\_development/fields)
  * metrolist\_development --> Development\_\_c
* MetroList - Unit (/admin/structure/salesforce/mappings/manage/metrolist\_unit/fields)
  * metrolist\_unit --> Development\_Unit\_\_c

**Sales Force Settings**:

* MetroList - Development (/admin/structure/salesforce/mappings/manage/metrolist\_development)
* MetroList - Unit (/admin/structure/salesforce/mappings/manage/metrolist\_unit)

**Sales Force Mapping Field**: docroot/modules/custom/bos\_content/modules/bos\_metrolist/src/Plugin/SalesforceMappingField/RelatedTermStrings.php

> **Troubleshoot Sales Force connection issues**
>
> If Drupal and Sales Force are not connecting or syncing please check the Authorization from Drupal to Sales Force (/admin/config/salesforce/authorize/list). You may need to Re-auth or even make a new connection if you need to connect to a lower development or testing environment on Sales Force. If you need access to an instance contact DND's Sales Force developer/administrator.
>
> If a single item is not syncing or if you need info about the Drupal to Sales Force connection you can view the list this admin page. If you edit the instance you then have the option to force pull or push the Drupal entity with the Sales Force Object. If there is an issue you should see an error message in the response. You can also find other useful info like timestamps and record ids.
