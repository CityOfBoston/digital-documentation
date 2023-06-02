---
description: >-
  The following Drupal Entities are created to warehouse/cache data which
  originates in Salesforce.
---

# BH Drupal Entities

{% hint style="info" %}
The following nodes have records which are populated (add, update and delete) by mappings between Salesforce and Drupal which are run each cron cycle.

The following taxonomies have been created and their list items are maintained manually by Drupal developers.  Taxonomy items can be added and deleted as needed, but usually will need adjustments to code to work as required.
{% endhint %}

### Custom Entities

<table><thead><tr><th width="126.33333333333331">Entity</th><th width="282">Name</th><th>Description</th></tr></thead><tbody><tr><td>node</td><td>bh_project</td><td>The primary content_type for a Building Housing Property. Contains meta data about the Project and links to updates, attachments, parcels etc.</td></tr><tr><td>node</td><td>bh_update</td><td>Contains information about updates to a project. This includes certain status changes, attached documents, links to community meeting records and comments from CoB Project Managers to insert into the timeline.</td></tr><tr><td>node</td><td>bh_meeting</td><td>Contains information about Community Meetings held by CoB with residents regarding Building Housing Properties. </td></tr><tr><td>node</td><td>bh_parcel</td><td>The official parcel number and top-level info - with GIS coordinates for the parcel.</td></tr><tr><td>node</td><td>bh_parcel_project_assoc</td><td>Possibly deprecated ?<br><em>I believe the parcel# is now saved in the field_bh_parcel_id field of bh_project.</em></td></tr><tr><td>node</td><td>bh_contact</td><td><em><strong>Deprecated (no data)</strong></em></td></tr><tr><td>node</td><td>bh_account</td><td><em><strong>Deprecated (no data)</strong></em></td></tr><tr><td></td><td></td><td></td></tr><tr><td>taxonomy</td><td>bh_project_stage</td><td><p>The overall project stage. (usually when project status = active).</p><p>Linked directly from <code>bh_project</code>.</p></td></tr><tr><td>taxonomy</td><td>bh_project_status</td><td><p>The status of the Project.</p><p>Linked directly from <code>bh_project</code>.</p></td></tr><tr><td>taxonomy</td><td>bh_funding_stage</td><td>The funding stage for the Project.<br>Linked directly from <code>bh_project</code>.</td></tr><tr><td>taxonomy</td><td>bh_project_type</td><td>The broad project type for the Project.<br>Linked directly from <code>bh_project</code>.</td></tr><tr><td>taxonomy</td><td>bh_project_update_type</td><td>Update type for a <code>bh_update</code>.<br>Linked directly from <code>bh_update</code>.</td></tr><tr><td>taxonomy</td><td>bh_property_type</td><td>The property type.<br>Linked directly from <code>bh_parcel</code>.</td></tr><tr><td>taxonomy</td><td>bh_public_stage</td><td><p>The Project stage as used in the timeline.</p><p>Linked directly from <code>bh_project</code>.</p></td></tr><tr><td>taxonomy</td><td>bh_neighborhood</td><td></td></tr><tr><td>taxonomy</td><td>bh_record_type</td><td></td></tr><tr><td>taxonomy</td><td>bh_disposition_type</td><td>List of disposition types - for use in map.</td></tr><tr><td></td><td></td><td></td></tr><tr><td>view</td><td>building_housing</td><td></td></tr><tr><td>view</td><td>bh_maps</td><td></td></tr><tr><td>view</td><td>building_housing_updates</td><td></td></tr><tr><td></td><td></td><td></td></tr><tr><td>page</td><td>buildinghousing/[propertyname]</td><td>This is the landing page for information about a property.<br>This is a customized page for the node <code>bh_project</code> and contains a timeline and information about parcels.</td></tr></tbody></table>

### Data Schema&#x20;

_As at March 2023_

<img src="../../../../.gitbook/assets/file.excalidraw.svg" alt="" class="gitbook-drawing">
