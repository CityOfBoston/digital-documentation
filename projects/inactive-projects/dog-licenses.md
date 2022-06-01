---
description: An overview of the currently inactive Dog Licenses project
---

# Dog Licenses

## Project Overview

The Dog Licenses app is a tool for applying for, renewing and looking up a dog license in the City of Boston. The app is owned by Animal Care/Control.

App is currently located here:  [https://www.cityofboston.gov/animals/licenseapp/](https://www.cityofboston.gov/animals/licenseapp/)

Yearly maintenance needed on this app: Dropdown for previous year needs to be updated yearly around the turn of the new year (January 1) so that people can reference a license number from the year prior.Dropdown for year needs to be updated yearly around the turn of the new year (January 1) so that people can reference a license number from the year prior.&#x20;

### Project Purpose

Improve the existing business process(es) associated with applying for new dog license or renewing/looking up an existing dog license.&#x20;

#### Current process:

**Tracking**: Essentially the [cityofboston.gov](http://cityofboston.gov/) sends Animal Care and Control an email, and staff manually enter information from that email into [Chameleon](https://chameleonbeach.com/).&#x20;

Chameleon is a software used by Animal Care and Control to track the licenses - all info that is submitted. __ Theoretically shares information nationally with any groups that also use the software and/or with major sites for lost pets, etc.

**Form Submission**: Custom asp.net form app.

**Payments**: Connects to Invoicecloud payments API - payment data sent via invoicecloud.

#### Desired process: [**https://drive.google.com/open?id=0B\_0Rju6iKR0aWXl0SzI1bUJ4LWc**](https://drive.google.com/open?id=0B\_0Rju6iKR0aWXl0SzI1bUJ4LWc)****

### Project History

05/07/2019 - Reilly checked with Susan Hynes - Susan couldn’t find any contracts in Peoplesoft financials for Chameleon or Chameleon beach

Animal Care and Control mentioned that Georges/Enterprise worked with Chameleon, so Jeanethe check with Joe/Goerges on this. Georges said the only one he knew that worked on a connection for animal licenses was Jeff Ng who helped connect [cityofboston.gov](http://cityofboston.gov/) to invoice cloud a few years ago. More on this here: See line 20 here: [https://docs.google.com/spreadsheets/d/1P6aBfR0rlltQgrCupTcvCvO70HzViQe6HxI3IiJWLXA/edit#gid=1207577308](https://docs.google.com/spreadsheets/d/1P6aBfR0rlltQgrCupTcvCvO70HzViQe6HxI3IiJWLXA/edit#gid=1207577308)

## Project next steps

### Things to explore:

* General discovery with Alexis/Animal Care and Control&#x20;
* Explore use of chameleon ([https://chameleonbeach.com/](https://chameleonbeach.com/)), i.e. software used by Animal Care and Control to track the licenses - all info that is submitted.
* Explore automation of manual entry of  information from emails (cityofboston.gov) into Chameleon. Can this be automated?
* Discrepancies between what is asked in person or via mail versus what is asked online. Note: There are differences - Example: We don't prompt for the aging population to get a discount like we do in person and via mail.
* Explore fee waiver use cases (e.g., do seeing eye dogs get fees waived?)
* Online payment. Is this another opportunity for Stripe? Currently connected to Invoice Cloud, but users get kicks to invoice cloud landing page.
* Explore merits of moving to Drupal or something else like forms, relative to keeping the app separate?

### Existing issues/obstacles:

* 'Breaks' from time to time due to miscellaneous issues

### Existing assets:

* We have feedback from constituents from feedback emails and customer satisfaction survey which is a Seamlessdoc > needs to be pulled/analyzed.&#x20;

**Project Plan**

Jeanethe is on deck to set up a time with Animal Care and Control for next steps

**PHASE I: DISCOVER**

* List of internal users and roles
* List of external users groups
  * Anyone needing to get a dog in Boston licensed, which
* User journey map
* For each group, a list of their needs, and whether/how they are being met by existing processes or technology
* Summarized insights from interviews with users (internal and external)
* Summarized information and supporting documentation on existing processes and technologies and any constraints they impose
* Survey of existing solutions (digital or not, in government or other industries)
* Metrics to determine what success looks like
* Hypotheses you will test during the prototyping phase

**PHASE II: PLAN**

* Project scope
* Preliminary project timeline, including milestones, deliverables, and resources needed at each stage

**PHASE III: PROTOTYPE**

* Define minimum viable prototype to test with users
* User testing plan and script
* Summarized findings from user testing

**PHASE IV: BUILD/BUY**

* Build vs. buy recommendation with supporting rationale
* If buying, summarized information on the vendor selected
* List of expected phases of implementation (e.g. staged releases or alpha version)
* Documentation on the technology or processes involved
* Plan for collecting and incorporating feedback
* Plan for publicizing the product

**PHASE V: IMPROVE**

* Notes from retrospective
* Feedback from users post-launch
* User metrics
* Plan going forward

