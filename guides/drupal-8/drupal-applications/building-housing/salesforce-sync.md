---
description: >-
  Drupal Building Housing records are synchronized from MOH SalesForce on a
  schedule. Salesforce is the authoritative source, and data should not be added
  or changed in Drupal.
---

# Salesforce Sync

There are 6 synchronizations with Salesforce which run in the following order, every cron run (so every 5 mins)  _The order is important, because Projects must be created before Attachments & Website Updates before Meetings & Chatter postings._

| Sync Name                                | Drupal Destination         | SF Origin                       |
| ---------------------------------------- | -------------------------- | ------------------------------- |
| Building Housing - Projects              | bh\_project                | Project\_\_c                    |
| Building Housing - Website Update        | bh\_update                 | Website\_Update\_\_c            |
| Building Housing - Project Update        | bh\_update                 | Update\_\_c                     |
| BH Community Meeting Event               | bh\_meeting                | Community\_Meeting\_Event\_\_c  |
| Building Housing - Parcels               | bh\_parcel                 | Parcel\_\_c                     |
| Building Housing - Parcels-Project Assoc | bh\_parcel\_project\_assoc | ParcelProject\_Association\_\_c |

### Building Housing - Projects

This synchronization imports Project records from Salesforce `Project__c` object into Drupals' `bh_project` entity.

<details>

<summary>Synchronization</summary>



</details>

<details>

<summary>Custom Sync Code</summary>

in `SalesforceBuildingHousingUpdateSubscriber.php:`

* `function pullQueryAlter` changes the SF query to include information on the Projects' Project Manager.&#x20;
* `function PullPresave` runs before the SF Query results are applied to Drupal and manually maps information about the Project Manager so it can be included in the `bh_project` record.

</details>

### Building Housing - Website Update

This synchronization manages project stages, documents and messages to appear on the timeline.  _It extends and replaces the functionality for the Update\_\_c object which is imported for legacy reasons in `Building Housing - Project Update`._

{% hint style="info" %}
There is only ever 1 Website Update (`Website_Update__c`) record per Project (`Project__c`) record in Salesforce.&#x20;

There is a rule in Salesforce to stop multiple records which would potentially create confusion for project stages etc.

**If multiple Website Update records do exist for a Project in Salesforce, then all records will be imported into Drupal, but ONLY the last (when ordered by createdDate) will be used in the** [**Project Timeline**](project-timeline.md)**.**
{% endhint %}

<details>

<summary>About Posts (aka Messages, Text Messages)</summary>

MOH Staff sometimes wish to add comments, information or documents (see _"About Attachments"_ section) onto the Projects Timeline in Drupal.&#x20;

This is done by making Chatter Posts attached to the Website Update (`Website_Update__c)` object in Salesforce.

There are a number of Chatter Post types - This synchronization processes `TextPost` and `ContentPosts` - all other Post types are disregarded.

Attachments included in Posts (`ContentPosts`) are not imported - only the text content is (text content in `TextPosts` is also always imported).\
_**See** attachments importing in the "About Attachments" section_.

All embellishments included in the Chatter Post (in-line images, links, emoji's and font styling) are stripped during processing so only text is retained.&#x20;

In Drupal, Posts are saved into an "array" in the `field_bh_text_updates` field in the `bh_update` record.

Custom code handles updating and deleting Posts.

In Salesforce, adding a text-only (`TextPost`) Chatter Post to a Website Update **does not** automatically trigger a sync.  In order to sync a Chatter Post, a MOH Staff member must manually save the Website Update record which then triggers a Drupal sync of the Website Update in turn causing the Posts to be scanned and processed.

</details>

<details>

<summary>About Attachments (aka Documents, Files, Images)</summary>

MOH Staff sometimes wish to add documents or images to the project and/or onto the Projects Timeline in Drupal

Documents and images can be added to a Website Update (`Website_Update__c)`in Salesforce by uploading an `Attachment` in the Website Update, or by creating a Website Update Chatter Post with an attached document.

_**Note:** There is a process on the Salesforce side which processes Website Update Chatter Posts and saves any attachments as Salesforce `Attachment` objects._

_**Note:** At the moment, PNG, JPG/JPEG images and PDF documents are properly handled and should open in the users browser.  Other document types will import and link to the timeline, but may not display reliably when clicked by a user in the Project Timeline (in Drupal) ._

In Drupal, Attachments are saved as `file` entities and are linked to both the `bh_project` entity and its associated `bh_project` entity.

Custom code handles updating and deleting Attachments.

In Salesforce, adding an Attachment (directly or via a Chatter Post) to a Website Update automatically triggers a sync.&#x20;

</details>

<details>

<summary>Synchronization</summary>



</details>

<details>

<summary>Custom Sync Code</summary>

in `SalesforceBuildingHousingUpdateSubscriber.php`:

* `function pullQueryAlter` changes the SF query to include information on the  Website Updates' Attachments, and the Attachment files.
* `function PullPresave` runs before the SF Query results are applied to Drupal and manually \
  \- manages Posts in `bh_update,`and \
  \- manages Attachments as `file` objects, and\
  \- links the `file` objects to the `bh_update` and the `bh_project` entities.

</details>

### Building Housing - Project Update

{% hint style="info" %}
There should be no new Update\_\_c records being created in SF. \
However, there are legacy records containing data which must be included in Drupal.  \
_Even though we do not normally expect the sync to process these objects, the code is important if the data is to be recreated accurately and completely (for example if a Salesforce purge is performed)._
{% endhint %}

This handles legacy TextMessages (now use chatter) and document attachments (now use Website Update Attachments).

<details>

<summary>Synchronization</summary>



</details>

<details>

<summary>Custom Sync Code</summary>

in `SalesforceBuildingHousingUpdateSubscriber.php`:

* `function pullQueryAlter` changes the SF query to include information on the  Project Updates' Attachments, and the Attachment files.
* `function PullPresave` runs before the SF Query results are applied to Drupal and manually\
  \- manages Posts in `bh_update,`and \
  \- manages Attachments as `file` objects, and\
  \- links the `file` objects to the `bh_update` and the `bh_project` entities.

</details>

### BH Community Meeting Event

This synchronization imports Community Meetings event records from Salesforce `Community_Meeting_Event__c` object into Drupals `bh_meeting` entity.

This is a simple mapping and the import does little except cleaning up any URLs and address fields.

The `bh_meeting`record holds a reference to it's parent `bh_update`which is linked to the `bh_project`.

If a Meeting event is updated, or deleted in SF, then the associated record will be updated in Drupal, and if necessary will move on the timeline.

<details>

<summary>Synchronization</summary>



</details>

<details>

<summary>Custom Sync Code</summary>

in `SalesforceBuildingHousingUpdateSubscriber.php`:

* `function PullPresave` runs before the SF Query results are applied to Drupal and manually cleans up URL's and addresses

</details>

### &#x20;Building Housing - Parcels

<details>

<summary>Synchronization</summary>



</details>

<details>

<summary>Custom Sync Code</summary>

None

</details>

### Building Housing - Parcels-Project Assoc

<details>

<summary>Synchronization</summary>



</details>

<details>

<summary>Custom Sync Code</summary>

None

</details>
