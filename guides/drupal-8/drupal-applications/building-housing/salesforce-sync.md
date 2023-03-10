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

### Building Housing - Website Update

This syncronization manages project stages, documents and messages to appear on the timeline.  It extends and replaces the functionality for the Update\_\_c object which is imported for legacy reasons in `Building Housing - Project Update`.

### Building Housing - Project Update

This handles legacy TextMessages (now use chatter) and document attachments (now use Website Update Attachments).

### BH Community Meeting Event

### Building Housing - Parcels
