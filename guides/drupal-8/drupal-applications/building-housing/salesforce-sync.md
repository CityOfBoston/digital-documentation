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

* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_project\_name direction: sf\_drupal salesforce\_field: Name id: 0 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_parcel\_id direction: sf\_drupal salesforce\_field: Parcel\_ID\_\_c id: 1 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_total\_num\_units direction: sf\_drupal salesforce\_field: Total\_No\_of\_Units\_\_c id: 2 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_project\_start\_date.value direction: sf\_drupal salesforce\_field: Project\_Start\_Date\_\_c id: 3 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_record\_type direction: sf\_drupal salesforce\_field: RecordTypeId id: 4 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_project\_stage direction: sf\_drupal salesforce\_field: Project\_Stage\_\_c id: 5 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_project\_status direction: sf\_drupal salesforce\_field: Project\_Status\_\_c id: 6 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_funding\_stage direction: sf\_drupal salesforce\_field: NHD\_Stage\_\_c id: 7 description: ''
* drupal\_field\_type: properties drupal\_field\_value: title direction: sf\_drupal salesforce\_field: Name id: 8 description: ''
* drupal\_field\_type: properties\_extended drupal\_field\_value: field\_bh\_coordinates.lat direction: sf\_drupal salesforce\_field: Parcel\_Y\_Coordinate\_\_c id: 9 description: ''
* drupal\_field\_type: properties\_extended drupal\_field\_value: field\_bh\_coordinates.lng direction: sf\_drupal salesforce\_field: Parcel\_X\_Coordinate\_\_c id: 10 description: ''
* drupal\_field\_type: properties\_extended drupal\_field\_value: field\_bh\_wysiwyg\_photo\_a.value direction: sf\_drupal salesforce\_field: Project\_Photo\_1\_\_c id: 11 description: ''
* drupal\_field\_type: properties drupal\_field\_value: body.value direction: sf\_drupal salesforce\_field: Narrative\_\_c id: 12 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_neighborhood direction: sf\_drupal salesforce\_field: Neighborhood\_computed\_\_c id: 13 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_disposition\_type direction: sf\_drupal salesforce\_field: Disposition\_Type\_\_c id: 14 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_project\_complete\_date.value direction: sf\_drupal salesforce\_field: Actual\_Finish\_Date\_\_c id: 15 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_initial\_comm\_mtg\_date.value direction: sf\_drupal salesforce\_field: Initial\_Cmnty\_Meeting\_Complete\_Actual\_\_c id: 16 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_rfp\_issued\_date.value direction: sf\_drupal salesforce\_field: RFP\_Issued\_Actual\_\_c id: 17 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_initial\_td\_vote\_date.value direction: sf\_drupal salesforce\_field: Initial\_TD\_Vote\_Actual\_\_c id: 18 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_dnd\_funding\_award\_date.value direction: sf\_drupal salesforce\_field: Award\_Enroll\_\_c id: 19 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_conveyance\_vote\_date.value direction: sf\_drupal salesforce\_field: Conveyance\_Vote\_Actual\_\_c id: 20 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_commitment\_letter\_date.value direction: sf\_drupal salesforce\_field: Commitment\_Letter\_Date\_\_c id: 21 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_predicted\_closing\_date.value direction: sf\_drupal salesforce\_field: Finance\_Closing\_\_c id: 22 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_construction\_start\_date.value direction: sf\_drupal salesforce\_field: Construction\_Start\_Date\_\_c id: 23 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_construct\_complete\_date.value direction: sf\_drupal salesforce\_field: Construction\_Finish\_Actual\_\_c id: 24 description: ''
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_project\_type direction: sf\_drupal salesforce\_field: Project\_Type\_\_c id: 25 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_property\_size direction: sf\_drupal salesforce\_field: Total\_Square\_Footage\_\_c id: 26 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_units\_total direction: sf\_drupal salesforce\_field: Total\_No\_of\_Units\_\_c id: 27 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_units\_affordable direction: sf\_drupal salesforce\_field: Total\_Units\_Income\_\_c id: 28 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_units\_for\_rent direction: sf\_drupal salesforce\_field: Rent\_\_c id: 29 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_units\_to\_own direction: sf\_drupal salesforce\_field: Own\_\_c id: 30 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_councilor direction: sf\_drupal salesforce\_field: Councilor\_\_c id: 31 description: ''

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

**Therefore Messages are imported into Drupal by the Website Update sync process.**

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

**Therefore Attachments are imported into Drupal by the Website Update sync process.**

_**Note:** There is a process on the Salesforce side which processes Website Update Chatter Posts and saves any attachments as Salesforce `Attachment` objects._

_**Note:** At the moment, PNG, JPG/JPEG images and PDF documents are properly handled and should open in the users browser.  Other document types will import and link to the timeline, but may not display reliably when clicked by a user in the Project Timeline (in Drupal) ._

In Drupal, Attachments are saved as `file` entities and are linked to both the `bh_project` entity and its associated `bh_project` entity.

Custom code handles updating and deleting Attachments.

In Salesforce, adding an Attachment (directly or via a Chatter Post) to a Website Update automatically triggers a sync.&#x20;

</details>

<details>

<summary>Synchronization</summary>

The Salesforce `Website_Update__c` object is linked to the Drupal `bh_update` entity:

* Project\_Goals\_\_c -> body.value&#x20;
* Name => title
* Boston\_gov\_Link\_\_c => field\_bh\_project\_web\_link
* Project\_\_c => field\_bh\_project\_ref
* Follower\_emails\_\_c => field\_bh\_follower\_emails&#x20;
* "1" => field\_sf\_web\_update
* Project\_Developer\_Formula\_\_c => field\_bh\_developer\_name
* Account\_Website\_\_c => field\_bh\_developer\_website&#x20;
* Boston\_Based\_Buiness\_\_c => field\_bh\_boston\_based\_business&#x20;
* Minority\_Owned\_Business2\_\_c => field\_bh\_minority\_owned\_business&#x20;
* Veteran\_Owned\_Business2\_\_c => field\_bh\_veteran\_owned\_business&#x20;
* Woman\_Owned\_Buiness2\_\_c => field\_bh\_women\_owned\_business&#x20;

**Note:** The field `field_bh_text_updates`in Drupal is updated with a string array of Chatter messages.

**Note:** A Drupal `file` object is created for each attachment found in the `attachment` field of the `Website_updates__c` object in SF.

**Note:** The field `field_bh_attachment` and/or `field_bh_project_images`in Drupal `bh_update` entity is updated with an array of files (attachments found in `Website_Updates__c.Attachments` in SF).

**Note:** The field `field_bh_attachment` and/or `field_bh_project_images` in Drupal `bh_project` entity is updated with an array of files (attachments found in `Website_Updates__c.Attachments` in SF).

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

* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_headline direction: sf\_drupal salesforce\_field: Headline\_\_c id: 0
* drupal\_field\_type: RelatedIDs drupal\_field\_value: field\_bh\_project\_ref direction: sf\_drupal salesforce\_field: Project\_\_c id: 1
* drupal\_field\_type: properties drupal\_field\_value: status direction: sf\_drupal salesforce\_field: Publish\_to\_Web\_\_c id: 2
* drupal\_field\_type: properties drupal\_field\_value: title direction: sf\_drupal salesforce\_field: Name id: 3
* drupal\_field\_type: properties\_extended drupal\_field\_value: body.value direction: sf\_drupal salesforce\_field: Update\_Body\_\_c id: 4
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_update\_type\_ref direction: sf\_drupal salesforce\_field: Type\_\_c id: 5
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_update\_posted\_date direction: sf\_drupal salesforce\_field: CreatedDate id: 6

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

* drupal\_field\_type: properties drupal\_field\_value: title direction: sf\_drupal salesforce\_field: Title\_\_c id: 0 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_meeting\_start\_time.value direction: sf\_drupal salesforce\_field: Meeting\_Date\_Time\_Start\_\_c id: 1 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_meeting\_end\_time.value direction: sf\_drupal salesforce\_field: Meeting\_Date\_Time\_End\_\_c id: 2 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_languages\_supported direction: sf\_drupal salesforce\_field: Languages\_supported\_\_c id: 3 description: ''
* drupal\_field\_type: properties drupal\_field\_value: body.value direction: sf\_drupal salesforce\_field: Meeting\_Description\_\_c id: 4 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_contact\_person\_sfid direction: sf\_drupal salesforce\_field: Contact\_person\_\_c id: 5 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_meeting\_goal direction: sf\_drupal salesforce\_field: Meeting\_Goal\_\_c id: 6 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_meeting\_recording\_pass direction: sf\_drupal salesforce\_field: Meeting\_Recording\_Password\_\_c id: 7 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_number\_of\_attendees direction: sf\_drupal salesforce\_field: Number\_of\_attendees\_\_c id: 8 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_post\_meeting\_notes direction: sf\_drupal salesforce\_field: Post\_meeting\_Notes\_\_c id: 9 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_post\_meeting\_recording direction: sf\_drupal salesforce\_field: Post\_meeting\_recording\_\_c id: 10 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_virt\_meeting\_web\_addr direction: sf\_drupal salesforce\_field: Virtual\_meeting\_web\_address\_\_c id: 11 description: ''
* drupal\_field\_type: RelatedIDs drupal\_field\_value: field\_bh\_update\_ref direction: sf\_drupal salesforce\_field: Website\_Update\_\_c id: 12 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_host\_organization direction: sf\_drupal salesforce\_field: Host\_Organization2\_\_c id: 13 description: ''
* drupal\_field\_type: properties drupal\_field\_value: field\_address.address\_line1 direction: sf\_drupal salesforce\_field: Address\_\_c id: 14 description: 'DU Aug 2022. Insert SF text address into first line of drupal address component.'

</details>

<details>

<summary>Custom Sync Code</summary>

in `SalesforceBuildingHousingUpdateSubscriber.php`:

* `function PullPresave` runs before the SF Query results are applied to Drupal and manually cleans up URL's and addresses

</details>

### &#x20;Building Housing - Parcels

<details>

<summary>Synchronization</summary>

* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_parcel\_name direction: sf\_drupal salesforce\_field: Name id: 0
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_parcel\_header direction: sf\_drupal salesforce\_field: Parcel\_Header\_\_c id: 1
* drupal\_field\_type: RelatedTermString drupal\_field\_value: field\_bh\_property\_type direction: sf\_drupal salesforce\_field: Property\_Type\_\_c id: 2
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_owned\_by\_dnd direction: sf\_drupal salesforce\_field: Active\_\_c id: 3
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_assessing\_lot\_size direction: sf\_drupal salesforce\_field: Assessing\_Lot\_Size\_\_c id: 4
* drupal\_field\_type: properties drupal\_field\_value: field\_bh\_street\_address\_temp direction: sf\_drupal salesforce\_field: Street\_Address\_\_c id: 5
* drupal\_field\_type: properties drupal\_field\_value: title direction: sf\_drupal salesforce\_field: Name id: 6
* drupal\_field\_type: properties\_extended drupal\_field\_value: field\_bh\_coordinates.lat direction: sf\_drupal salesforce\_field: Y\_Coordinate\_\_c id: 7
* drupal\_field\_type: properties\_extended drupal\_field\_value: field\_bh\_coordinates.lng direction: sf\_drupal salesforce\_field: X\_Coordinate\_\_c id: 8

</details>

<details>

<summary>Custom Sync Code</summary>

None

</details>

### Building Housing - Parcels-Project Assoc

<details>

<summary>Synchronization</summary>

* drupal\_field\_type: RelatedIDs drupal\_field\_value: field\_bh\_parcel\_ref direction: sf\_drupal salesforce\_field: Parcel\_\_c id: 0
* drupal\_field\_type: RelatedIDs drupal\_field\_value: field\_bh\_project\_ref direction: sf\_drupal salesforce\_field: Project\_\_c id: 1
* drupal\_field\_type: properties drupal\_field\_value: title direction: sf\_drupal salesforce\_field: Name id: 2

</details>

<details>

<summary>Custom Sync Code</summary>

None

</details>
