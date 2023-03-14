---
description: >-
  The largest on-page component for a Building Housing Project record in Drupal
  is its timeline. The timeline visually displays a Projects past and projected
  events & information in a time ordered list.
---

# BH Project Timeline

The main code is contained here:

`docroot/modules/custom/bos_content/modules/node_buildinghousing/src/Plugin/Field/FieldFormatter/EntityReferenceTaxonomyTermBSPublicStageFormatter.php`

Various templates are defined to create each timeline article, one template per timeline type.&#x20;

### Basic Strategy

1. Timeline items are gathered from various places in the system.\
   \- **text posts** are items in the `field_bh_text_updates` field of the `bh_update` entity. Tiles are inserted using the messages create date for ordering.  If the message is updated in SF, it continues to use the original created date. If the message is deleted in SF it should be deleted from the timeline. Posts are sourced from Salesforce Chatter and imported using [this synchronization](../bh-salesforce-sync/#building-housing-website-update).\
   \- **documents** are file objects which are referenced as items in the `field_bh_attachment` field of the `bh_project`. Tiles are inserted using the create date of the attachment for ordering. _If_ Drupal detects an update to the attachment in SF, it continues to use the original created date for ordering. Documents are sourced from Salesforce and imported using [this synchronization](../bh-salesforce-sync/#building-housing-website-update).\
   \- a single **rfp** tile is created if the `field_bh_rfp_issued_date` field is not empty  Tile is inserted using the date in the `field_bh_rfp_issued_date` field. If the date is changed (or deleted) in SF, the rfp tile will be moved or removed from the timeline. RFP date is sourced from Salesforce as part of [this synchronization](../bh-salesforce-sync/#building-housing-website-update).\
   \- **meetings** are `bh_meeting` objects which are referenced as  items in the `field_bh_update_ref` field of the `bh_update` entity. Tiles are inserted using the meetings start date.  If the meeting is updated in SF, meeting will move accordingly in the timeline. Meetings are sourced from Salesforce and imported using [this synchronization](../bh-salesforce-sync/#bh-community-meeting-event).\
   \- **stages**
2. Timeline icons are controlled by css and are defined in function `getStageIcon()` in `EntityReferenceTaxonomyTermBSPublicStageFormatter.php`
3. The various stages are defined at items in the `bh_public_stage` taxonomy&#x20;
4.



