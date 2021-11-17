# Proposed Restructure

Use Drupal as core.

* Drupal content type manages the email subscription list
* Drupal is used to render subscription forms etc as webpages on boston.gov
* Cron in drupal manages communications dispatches.
* Cron in drupal manages sync of Street sweeping (SAM) records and Street Occupancy (Hansen) records.

Use PostMark as the email delivery mechanism.

Retain Twillio for SMS (and voice?)

Use either existing MSSQL DB (COB Datacenter) or else port to MySQL (on Acquia)
