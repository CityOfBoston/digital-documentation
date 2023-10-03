# Salesforce Contributed Module

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

### Troubleshooting Salesforce Connection

> **Troubleshoot Sales Force connection issues**
>
> If Drupal and Sales Force are not connecting or syncing please check the Authorization from Drupal to Sales Force (/admin/config/salesforce/authorize/list). You may need to Re-auth or even make a new connection if you need to connect to a lower development or testing environment on Sales Force. If you need access to an instance contact DND's Sales Force developer/administrator.
>
> If a single item is not syncing or if you need info about the Drupal to Sales Force connection you can view the list this admin page. If you edit the instance you then have the option to force pull or push the Drupal entity with the Sales Force Object. If there is an issue you should see an error message in the response. You can also find other useful info like timestamps and record ids.
