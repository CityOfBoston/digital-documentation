---
description: >-
  Building Housing allows constituents to see a full inventory of projects and
  parcels managed by the city of Boston.
---

# BH Property Webpage

### **Overview**

**Entry point**: /buildinghousing (click show map)

**URL pattern**: /buildinghousing/{ProjectName}

**Custom CSS**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/css/node\_bh\_project.css

**Field Templates**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/templates/snippets

**Field Formatters**: docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/Plugin/Field/FieldFormatter

**Helper Functions** (Pre-process, alters, fields):

* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/node\_buildinghousing.module
* docroot/modules/custom/bos\_content/modules/node\_buildinghousing/src/BuildingHousingUtils.php

### Page Elements

![](../../../../../.gitbook/assets/BuildingHousing\_ProjectPage.png)

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
   * [More Information](bh-project-timeline.md)
8. **Project Type** - Custom field in node\_buildinghousing.module
9. **Contact information** - Custom field in node\_buildinghousing.module
10. **Email sign-up** - Custom field in node\_buildinghousing.module
11. **Feedback form** - settings

### The Project Timeline

