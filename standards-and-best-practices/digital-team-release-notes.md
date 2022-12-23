---
description: Code release details will be documented with each code deployment here
---

# Digital Team Release Notes

## Digital\_Tag/2022\_12\_23

DIG-Error message in search of Permit Finder website

* This fixes a bug where users where seeing an 400 error when using the permit finder website&#x20;

## Drupal\_Tags/2022\_12\_23

**Digital Team**&#x20;

DIG-1545 - Adjust feedback link in navigation for pages where feedback form appears

* This adds functionality to the feedback link seen the on top of Boston.gov pages. If a Boston.gov page has the newly created feedback embedded when the feedback link is clicked the user will navigate directly to the new feedback form at the bottom of the page&#x20;
* If there is no feedback form embedded on the page, when the feedback link is clicked the user will see a contact form that submits an email to [311supervisors@boston.gov](mailto:311supervisors@boston.gov)&#x20;

DIG-1307 - Add updated styling to tables on Boston.gov

* This updates all the tables on Boston.gov to match the newly designed tables created for the Elections results. These new designs are for both desktop and mobile. These updates were created to align with our commitment to design and brand consistency on Boston.gov

**Metrolist**&#x20;

DIG-1547 Make Availability Info Page Required

* Removes the check box option on the “Select Building” Page of the listing form that allows the users to skip the “Availability Info” page. This step will now be required in the submission process on the Metrolist Listing Form

## Drupal\_Tags/2022-12-14&15

**Metrolist**&#x20;

DIG-1047 Open Project Pages in New Tab

* Fixes a navigation issue for users that want to open a project page from the Build In Boston map. When clicking on a project from the map the project page will open in a new tab allowing users the ability to toggle between the map and project tabs they are viewing, and close the project pages without closing the map.&#x20;

**Digtial Team**&#x20;

DIG-1591 - Dupal Updates

* Routine maintenance and code release cycle for boston.gov

## Drupal\_Tags/2022-12-08

**Digital Team**&#x20;

DIG-1463 Create Boston.gov feedback form

* Enhancement in Drupal to create a feedback form that editors have to the ability to  add to the bottom of any page on Boston.gov
* Feedback form works on both desktop and mobile
*   &#x20;The feedback form has the following features:

    * Yes/No check boxes for user experience - Required field
    *   A comment text box for users to share their experience in paragraph form



        ![](<../.gitbook/assets/image (4).png>)\




    DIG-1544 'Notes' error when viewing feedback form submissions&#x20;
* Fixes a bug where users were seeing an error when they clicked the notes option on individual Drupal pages

**Metrolist**

DIG-777 Listing Form: Availability Info Screen

The following updates were made to the Availability Screen on the Metrolist form:

* Set default time in Deadline Time field to 11:59PM
* Add the “**Remove Posting Date**” date field so users can add a date to indicate when a posting should be removed so applications cannot be submitted after that date
* Updated the name of the “When would you like this posted to Metrolist“ field to “Available On”

DIG-838 Equitable Treatment agreement

Three changes to the notification requiring equitable treatment and non-discriminatory practices agreement:

1. Moved to bottom of page, just above submit button
2. Added "I agree" checkbox as part of notification
3. Disabled "Submit" button until "I agree" has been checked

DIG-1040 Fix Pagination icons to show displayed page&#x20;

* This updates the Metrolist Listing form so the user sees the pagination icon highlighted to indicate which page they are currently on&#x20;

## Digital\_Tags/2022-12-02

DIG-1024 View Only Group Management&#x20;

* Enhancement to the Group Management Tool on Access Boston Portal that allows users to search and view employees/contractors list of security groups in view only mode
* Users with the following security group SG\_AB\_GROUPMGMT\_SERVICEDESKVIEWONLY will see the Group Management link on their Access Portal page&#x20;
* Users will have the ability to search on users by name or ID&#x20;
* Once the correct user is found and selected their security groups will be displayed in view only mode. No edits can be made to the security groups

## Drupal\_Tags/2022-12-01

DIG-878 'Grid of quotes' component image upload issues

* This fix allows users to upload an image by easily clicking the “media add page” in the "Grid of Quotes" component, instead of forcing them to add the media then searching to to find it so it displays
* This also fixes the display itself so that the image can been seen instead of displaying the file name

DIG-1362 Display unofficial results in the same order as the ballot

* This fix allows user to see the unofficial election results data displayed in the same order as it is listed on the official election ballot when it appears on the unofficial elections results page&#x20;
* The election data order should also display in the same order as the ballot in the filtered drop down for searching

DIG-1393 Add field to Drupal to allow election editors / admins to edit disclaimer

* This fix gives election editors or admins the ability to update the disclaimer message on the Elections Results page&#x20;
* The editor will see a new field to add or update a disclaimer message in Drupal&#x20;

DIG-1435 Error on elections file upload crashes upload form

* Fixes an issue to prevent upload crashes when an elections file is uploaded
* To fix this issue the following solutions were implemented:&#x20;
  * Made the form more tolerant to missing or orphaned data in the history object which is dynamically stored in the node\_elections config settings.
  * Added a clear history button to the form so an admin can manage the history&#x20;
  * Added logging into the history so that clearing and deleting history is recorded&#x20;

DIG-1511: Maintenance Updates

* Routine contributed module updates for Drupal&#x20;

DIG- 1519 Content authors / editors unable to see drafts of unpublished content

* Fixes a bug where editors were unable to see their Draft Drupal pages, When a user saves a “draft” for a new content type, they get a “temporarily unavailable” message
* After an update in Drupal the DateTime module seemed to be less tolerant to formatting a date, the code was updated so that the published date will only be formatted if the node has been published

## Digital\_Tags/2022-11-09

DIG-62 Unmask a Password - Sign in Screen Access Boston Portal&#x20;

* Added a show password feature to the sign in screen on Access Boston Portal. This allows users to click on the word 'Show' to unmask the password they are typing to make sure it is correct

**NOTE:** This code was developed by the digital team but released by the IAM team because this page lives on their servers

![](<../.gitbook/assets/image (2).png>)

DIG-1155 Unmask a password - Change Password Screen&#x20;

* Added a show password feature to the Change Password screen on Access Boston Portal. This allows users to click on the word 'Show' to unmask the password they are typing to make sure it is correct
* This screen is also used in the create password process for new users/employees

![](<../.gitbook/assets/image (3).png>)

DIG-1156 - Unmask Password - Forgot Password Screen

* Added a show password feature to the Forgot Password screen on Access Boston Portal. This allows users to click on the word 'Show' to unmask the password they are typing to make sure it is correct

![](../.gitbook/assets/image.png)



## Drupal \_Tags/2022-11-03

* DIG- 1363 - Add disclaimer message to top of unofficial election results

Added a disclaimer message to the top of our unofficial election results election card to clarify the order that results appear for users.

* DIG -1374 - Add error validation in election uploads

This addresses an issue with uploading xml files in our new Election Results section in Drupal. It adds further error validation for users uploading problematic files.

## Patterns\_Tags/2022-11-03

* DIG- 1343-Adjust styling of tables on mobile

Adjusted the styling of our tables when viewing them on mobile in our patterns library. This change eliminates adding an extra border at the bottom of each cell, and instead adds the bottom border below groups of data on mobile.

## Drupal\_Tags/2022-11-01

DIG - 1333 Adjust text in filtered dropdown for primary elections

* Capitalized "rep" and "dem" party descriptors in the race selection dropdown&#x20;

## Drupal\_Tags/2022-10-28

### Unofficial elections results are mobile-friendly on Boston.gov

#### ****[**Epic: DIG- 943**](https://bostondoit.atlassian.net/browse/DIG-943) ****&#x20;

**Background:** The [unofficial election results website](https://www.boston.gov/departments/elections/unofficial-election-results) gets updated with new data each election that reflects the current race. The data for the current page is currently being iframed into a website on [Boston.gov](http://boston.gov/). The data from the iframe is coming from the old [cityofboston.gov](http://cityofboston.gov/) website. It does not have City of Boston branding and it isn’t mobile-friendly.

**Goal:** Make unofficial election results mobile-friendly on [Boston.gov](http://boston.gov/)

**UI Design**

[Mobile Version](https://xd.adobe.com/view/829947d0-12ec-4bc8-9b4e-30c924bfd551-d7ca/)

[Desktop Version ](https://xd.adobe.com/view/829947d0-12ec-4bc8-9b4e-30c924bfd551-d7ca/)

**Related Tickets**&#x20;

DIG- 1004 Importing Elections Data into Drupal&#x20;

* Created a new content type packaged in the module node\_elections allowing the elections dept to upload elections results into a drupal page
* Created an import page for the elections department to import election results file that will appear on the elections website
* Created an import process where election results file contents are loaded into the new content type to updated the elections results site

DIG- 1093 Create display pages for unofficial results

* Front end development work to create display pages for election results that align with approved UI designs and our patterns library



## Drupal\_Tags/2022-10-17

DIG-1206 Drupal Security update 9.4.8



## Patterns/2022-10-07

DIG-854 Update patterns library to Node 18&#x20;

## Drupal\_Tags/2022-09-23

DIG-1060 - Fixes a bug where the postmark contact form was not automatically adding the correct email address in the CC field so Boston City Workers could click reply all without having to cut and paste the email address into the To field. This fix allows users to see the email address in the To field after click reply or reply all.&#x20;

## Drupal\_Tags/2022-09-21

DIG-881 - This updates the my neighborhood look up tool by swapping out the summer links for the winter links.&#x20;

DIG-2021 -Routine scheduled updates to Drupal contributed modules

## AWS AMI Update/2022-09-21

* Weekly Maintenance that updates both PROD and the REPO

## Registry\_Tags/2022-09-20

DIG-1002 Error message in Registry suite of applications

* Fixes issue where users are getting a 400 error in Registry Suite of Applications
* Issue was due to a malformed cookie

## Drupal\_Tags/2022-09-20

DIG-1009 Internal links considered "external" causes WSD on older pages

* Fixes issue where Cabinet page links were broken&#x20;
* Updated code to ignore hard coded part of the URL and read the remainder of the path to display the correct page&#x20;

&#x20;

## Drupal\_Tags/2022-09-19

DIG-993 Contact forms on Boston.gov failing to send

* Fixes issue where users were unable to send emails via the "mail to" links on Boston.gov&#x20;
* A class was not registered for the email sending process via postmark

DIG-872 Add 'last updated' to 'updated' date in 'posts'

* This adds "Last Updated" before dates on a published Boston.gov page so users know when Boston.gov page has last been updated

DIG-949 Verify pages to be scanned by Percy

* Added a good selection of pages from Boston.gov for Percy testing



DIG-1005 Internal links considered "external" causes WSD on older pages

* Fixes issue where a URL link cannot resolve because drupal considers is external&#x20;
* Fix is to check if the link is external before trying to load the associated node&#x20;





## BoardsandCommissions\_Tag/2022-09-16

DIG-905 Apply button error in 'commission summary' component

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
