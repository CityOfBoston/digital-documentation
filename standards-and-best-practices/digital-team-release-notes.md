---
description: Code release details will be documented with each code deployment here
---

# Digital Team Release Notes

## BoardsandCommissions\_Tag/2022-09-16

DIG-905

* Fixes a 400 error users see when they click the Apply Online button on a Boards and Commission page
* Issue was due to a malformed cookie

## Drupal\_Tags/2022-09-16



DIG-67 -Caching issue displaying incorrect breadcrumbs

* This fixed an issue where the breadcrumbs on the top of a boston.gov page was not consistent with user navigation
* The solution is to specify that the breadcrumb block be cached per url and not per content type
* The breadcrumbs now appear consistent to the users navigation path

DIG-831 (metrolist)- Clarify "Minimum income" is annual income

* Added a tooltip to the “Minimum Income” field explaining that the amount entered in this field should be annual income

DIG -925 (metrolist)- Email Language Errors

* Updated confirmation email with correct language for the user

DIG-989 - Fix subdomain redirect in configuration for rentsmart.boston.gov\


* Updated our config file to point the [rentsmart.boston.gov](http://rentsmart.boston.gov/) redirect to [www.boston.gov/rentsmart](http://www.boston.gov/rentsmart)&#x20;



## Drupal\_Tags/2022-09-15

DIG-31 Custom 500 Error&#x20;

* This will activate the 500 error page on Boston.gov.&#x20;
* When a user gets a 500 error they will see the following:&#x20;

\
Text: _Sorry! Looks like something went wrong on our end. We're currently working to fix the issue. You can try re-loading the page in a few minutes, or email_ [_feedback@boston.gov_](mailto:feedback@boston.gov) _with any questions or concerns._

__![](<../.gitbook/assets/image (1).png>)__

__

DIG-853 Re-integrates Percy&#x20;

* This work re-enables Percy on Boston.gov
* Percy will assist in automated testing by comparing screenshots to ensure new code does not break anything on Boston.gov
* Percy tests must pass in order to merge new PRs

DIG-876 Resolves internal link WSD

* This fixes a validation error for broken internal links in components
* If a user enters an internal URL the page will resolve itself and navigate to the correct URL when saved
* When users try to save a draft page with a broken internal link they will get an error message.



## Drupal\_Tags/2022-09-07

DIG-542(Metrolist) Calendar events do not show physical location

* This will update calendar events to pull in the physical address of the event so users see the correct address on the site
* This update will also accommodate events that will be held virtually - users will see language on screen saying the event will be virtual

DIG-738(Metrolist) Stop re-submission of form once submitted

* Update to add language to the Metrolist listing form informing users the unique link they receive to submit a listing should only be used once. The following language has been added to these screen/emails
  * Listing Form Request - ** **_**Important: If you need to submit listings in multiple properties, please request a new form for each one.**_
  * Email communication -_**Important: Do not reuse link. If you need to submit listings for additional properties, please request a new form**_
  * Listing form request on screen notification- _**Important: If you need to submit multiple listings, please request a new form for each building.** _&#x20;

DIG-776(Metrolist) Submission Confirmation Email

* Send a "Submission successfully completed" email notice to the contact associated with the listing when a submission is completed

DIG-775(Metrolist) Update Admin email alert

*   Update the Admin email recipient list, add meaningful info, and include a link to the Salesforce Development. Email will contain:&#x20;

    | <p>Submitted on: [Date/time stamp]<br>Submitted By: [Listing Contact Name]<br>Listing Contact Company: [Listing Contact Company]<br>Contact Email: [Listing Contact Email]<br>Contact Phone: [Listing Contact Phone]</p><p>Property Name: [Development Name]<br>Property Address: [Street, City, Zip]</p> |
    | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

DIG-781(Metrolist) Apply Button on listings gets 404 Not Found

* Fixed bug where Apply button on MetroList Listing page was getting a 404 page error

DIG-809(Metrolist) Issues accessing Metrolist from external devices&#x20;

* Fixed bug where Metrolist search page was not loading on some devices&#x20;
* In the listing date code \T was being read as timezone updating this to \\\T fixed the issue&#x20;

DIG-433 (Metrolist)Telephone number (\&date) format on metrolist\_listing webform

* Fixed bug where user sees a formatting error after entering a phone number&#x20;

DIG-824 Update BOS311 API for Chinese translation

* Chinese translation was not appearing in BOS:311 App
* API was updated to send correct language code so alerts show in Simplified Chinese &#x20;

DIG- 865 Fix styling of events component in how-to page

* Fixed styling on events and notices component so events boxes are not pushed to right of the screen and appear centered on the site

## Drupal\_Tags/2022-8-24

DIG-317: How-to pages broken components&#x20;

* Fix styling issues with How To content type from patterns library&#x20;

DIG-318: Node landing page Full&#x20;

* Minor edit to the wrapper around the main items

DIG-319: Node Listing Page

* Took out unnecessary code to make sure listing pages look correct

DIG-438: Add email re-verification field.&#x20;

* Added validation email field to Postmark contact form so users have to validate their email before submitting a question etc.

DIG-757: Event calendar bleeding into the bottom module&#x20;

* Fixed a bug where the calendar button was bleeding onto the components section in events, this has been aligned&#x20;

DIG-733: Features sidebar svg&#x20;

* Fix icon svg files appearing too large in the features section on the ArtsBoston Calendar. They are now appearing at normal size

DIG-781: Apply button on listings gets 404

* Fix the apply button on Metrolist listing so when clicked user can apply to any listing&#x20;

## AccessBoston\_Tags/2022-08-18

DIG-65 - Group Management URL Security Fix&#x20;

* Fixes the following security issue of allowing users to access group management via copying a URL directly into the browser&#x20;
* Users that do not have access to group management will be directed back to the Access Boston Portal screen
