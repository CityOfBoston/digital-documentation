---
description: >-
  A No Tow subscription service where emails can be sent to registered
  constituents reminding them of sweeping days for a nominated street.
---

# Street Sweeping Reminders

## Summary

Residents can use the page at [https://www.cityofboston.gov/publicworks/sweeping/](https://www.cityofboston.gov/publicworks/sweeping/).

First the resident searches for their street or a calendar.  Matching street results are returned from a master schedule/calendar and the resident nominates which streets should be included in the alert.  The resident then supplies an email address and a preferred email reminder time.

Subscription from this page registers the resident for street sweeping reminders, and also for Street Occupancy alerts.

Emails are managed by a Lyris email server installed on the city network.

This service is active, there appear to be around 5 new subscriptions per day.  There are 100,488 (as at 2021-10-01) current active subscriptions, but many may be old and refer to defunct emails.  Lyris should be able to provide bounce reports. &#x20;

_**If the list is managed in Lyris, then entries removed in Lyris should be manually removed from the PwdSweepingEmails table or the remindme.asp (see Subscription Search) will be inaccurate.**_

## Code

This is an ASP application hosted on **ZPCOBWEB01.web.cob** (a DMZ IIS Server).

The code is is found in the following folder:

```
D:\wwwcob\publicworks\sweeping
```

### : web.config

This is the configuration file and this contains the database credentials.

### : default.asp

This page contains a list of cancellations.  It looks like known sweeping holidays (e.g. Veterans day) are loaded for the year in advance, and then ad-hoc cancellations (e.g. Snow or Ice) are loaded as the cancellation is advised by PWD.  Cancellations for general street sweeping and overnight sweeping are maintained separately.

The search form for the registration process is contained on this page.  The search is conducted against the `PwdSweeping` table in the `Towing` database on vSQL01 (aka ZPDMZSQL01).

The residents subscription choices (streets & dates) are managed by this page for use in the subscription process.

Subscription is initiated on this page but the actual subscription process is handed off to `subscribetostreet.asp` ([see below](street-sweeping-reminders.md#subscribetostreet-asp)).

Also the user can download a file which can be imported into calendars supporting the iCalendar format (this is most calendars). The file generation and download is managed by `addtocalendar.ics.asp` ([see below](street-sweeping-reminders.md#addtocalendar-ics-asp))

{% hint style="danger" %}
**Announcements column has a very outdated entry for 2013.**
{% endhint %}

### : subscribetostreet.asp

This page controls subscriptions and unsubscriptions.

_Subscriptions: _

Subscribing involves first removing and then adding the residents email and street into the table `PwdSweepingEmails` in the `Towing` database on vSQL01 (aka ZPDMZSQL01).

* if this is a first time subscription, the user is added or removed from the Lyris email server at [http://listserv.cityofboston.gov/subscribe/subscribe.tml](http://listserv.cityofboston.gov/subscribe/subscribe.tml).
* If this is not a first time subscription, then the users time preference is added to the `members_` table in the `Lyris` database on vSQL01 (aka(ZPDMZSQL01).

_Unsubscriptions:_

Unsubscribing involves removing the residents email and street from the table `PwdSweepingEmails` in the `Towing` database on vSQL01 (aka ZPDMZSQL01).

### : addtocalendar.ics.asp

_The iCalendar (ics) format is presently supported by: Google Calendar, Apple Calendar (formerly iCal), IBM Notes (formerly Lotus Notes), Yahoo! Calendar, Evolution (software), eM Client, Lightning extension for Mozilla Thunderbird and SeaMonkey, and partially by Microsoft Outlook and Novell GroupWise_

This page extracts a schedule from the `PwdSweeping` table in the `Towing` database on vSQL01 (aka ZPDMZSQL01) and formats into an ics format and then downloads the ics file to the residents computer.

## Database

{% hint style="info" %}
The utility code which manages connections to the Database Server and posts queries etc to the server is contained within:

`D:\wwwcob\includes\stdf.inc`
{% endhint %}

The database server used by this sub-service is **vSQL01** (aka ZPDMZSQL01).  The server is hosted in the DMZ in the web.cob (aka lincdom) domain.  Developers need a separate and specific account to be set up on the web.cob domain to view/edit databases and tables on this server.

### Towing

The main database used by this sub-service is **Towing.**

It appears that the code uses a trusted connection to the database server. To connect to this database you first need to have an account in the lincdom domain.  Then you need to have the Microsoft SQL Server Management Studio installed on your work PC. Then you need to connect to the server at **zpdmzsql01.web.cob** using your lincdom account (user:LINCDOM\username + password:userpassword).  This should work.

The tables used by this sub-service are:

| TableName         | Key Fields                                                                                                                                | Description                                                                                                                                                                                                                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PwdSweeping       | <ul><li>MainID</li><li>St_name</li><li>Dist(rict)</li><li>StartTime</li><li>EndTime</li><li>Side</li><li>...<br>[schedule info]</li></ul> | <p>Contains street sweeping schedule information for streets in the city.</p><p></p><p>It is unknown how this table is maintained.</p>                                                                                                                                          |
| PwdSweepingEmails | <ul><li>EmailAddr</li><li>StreetID</li></ul>                                                                                              | <p>Contains information on who has subscribed to what.</p><p><br>StreetID maps to the MainID in the PwdSweeping table.<br></p><p>This table is maintained by the scripts in this sub-service.  It is also used by the <strong>Subscription Search </strong>(aka remind me).</p> |

PwdSweeping is maintained elsewhere and is active - The last update (as at 2021-10-01) was 2021-09-07.  \[**Check with Satyen on how the data gets into that table**]

### Lyris

There is a database called **Lyris** on the same server (vSQL01). &#x20;

It seems that the Lyris mail server uses the `Lyris` database on vSQL01. It also seems the recipients and their send-time preferences are described in the `members_` table.  Further, Lyris seems to get the streets the members are subscribed to from the `PwdSweepingEmails` table. &#x20;

The Lyris server is available on `https://listserv.cityofboston.gov` login  can be provided by James Duffy and will be your city email address and a password.  If you are a Server Administrator you will be able to configure the actual Lyris service.

The list used is named `no-tow` and separate Segments are created for the street sweeping "groups" (i.e. groups of streets that get cleaned at the same time).  Users are added to a Segment based on their subscription selections.

## Connected Services

### Lyris

Lyris is used for handling the street sweeping emails. This is an in-house email server (a mailing list server) which has an API at [http://listserv.cityofboston.gov/subscribe/subscribe.tml](http://listserv.cityofboston.gov/subscribe/subscribe.tml).  The list used is `no-tow`.

