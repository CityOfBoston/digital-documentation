---
description: >-
  This app initiates and manages inquiries from the public on existing workers'
  compensation claims filed with the City.
---

# Workers' Compensation Inquiry From

{% hint style="info" %}
**This is a form and data grid C# app hosted on ZPCOBWEB01 (cityofboston.gov IIS server in DMZ).**
{% endhint %}

### App Overview

The application is found in the folder at /wwwcob/workerscomp/inquiryform.

The normal workflow is:

1. Inquiry is filed by a member of the public at workerscomp/inquiryform.aspx.&#x20;
   1. The inquiry is saved in a database table workerscomp.inquiries in VSQL01
   2. An email is returned to the inquirer
2. The workers compensation team login to workercomp/inquiryform/inquiryadmin.aspx and review the request.
   1. They can make notes and mark the inquiry as open or closed
   2. They can respond to the inquirer using (email sent via /workerscomp/inquiryform/response.aspx

### Dependencies

#### Database

The database server used is on **VSQL01** (DMZ database), and the database is **workerscomp.**&#x20;

#### Emails

As of March 2023, emails are now routed through **justice.cityhall.boston.cob**. \
(prior to 2023 updates ZSMTP01 - smtp.web.cob was used)
