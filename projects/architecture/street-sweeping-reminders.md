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

### Statistics

_Lyris maintains subscribers (members) in its **no-tow** list._

| Statistics as at 10/27/2021  | Value                                                               |
| ---------------------------- | ------------------------------------------------------------------- |
| Total List                   | <p>39,218 (lyris)</p><p>101,139 (PwdSweepingEmails - Towing DB)</p> |
| August 2021 subscriptions    | 317                                                                 |
| September 2021 subscriptions | 473                                                                 |
| August 2021 Unsubscribe      | -258                                                                |
| August 2021 unsubscribe      | -256                                                                |

![](<../../.gitbook/assets/image (28).png>)![](<../../.gitbook/assets/image (29) (1).png>)

**Note:  **_Lyris subscribes street occupancy and street sweeping members to the same **no-tow** list._

_**Note: **Lyris manages its list members and removes those which cannot be delivered to.  Hence the numbers in Lyris are lower than in the Towing DB tables._

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

### :lyris.asp

This page is a microservice endpoint managed by IIS/ASP.&#x20;

The endpoint is designed to be called with an email address and date in the querystring. The script will return nothing if the email recipient is not scheduled to receive an email, and a full html if the recipient is due an email (according to the streets and Email time preference settings).

```
?email=david.upton@boston.gov&date=3/20/2021
```

The script runs sql statements against the `PWDSweepingEmails`, `PWDSweeping` and `PwdDist` tables in the `Towing` database on vSQL01 and applies logic to determine if an email is required for that recipient.

See Lyris below in Connected Services.

{% hint style="success" %}
**The body text for the Street Sweeping reminder emails is set in this script.**
{% endhint %}

{% hint style="info" %}
The script can be manually edited and advance cancellation dates can be added. &#x20;

If the logic determines that a recipient should receive an email, and a matching date is found in the list of cancellations a note is added to the reminder that sweeping is cancelled for the day/night.&#x20;

This is useful for planned events - city holidays (e.g. Christmas), but if cancellation is known sufficiently far in advance or the cancellation is for an extended period (e.g. snow storms) then it could be used to advise subscribers of the cancellation.

* _but remember emails remind about sweeping the next day _
{% endhint %}

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

| Table Name        | Key Fields                                                                                                                                | Description                                                                                                                                                                                                                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PwdSweeping       | <ul><li>MainID</li><li>St_name</li><li>Dist(rict)</li><li>StartTime</li><li>EndTime</li><li>Side</li><li>...<br>[schedule info]</li></ul> | <p>Contains street sweeping schedule information for streets in the city.</p><p></p><p>It is unknown how this table is maintained.</p>                                                                                                                                          |
| PwdSweepingEmails | <ul><li>EmailAddr</li><li>StreetID</li></ul>                                                                                              | <p>Contains information on who has subscribed to what.</p><p><br>StreetID maps to the MainID in the PwdSweeping table.<br></p><p>This table is maintained by the scripts in this sub-service.  It is also used by the <strong>Subscription Search </strong>(aka remind me).</p> |

PwdSweeping is maintained elsewhere and is active - The last update (as at 2021-10-01) was 2021-09-07.  \[**Check with Satyen on how the data gets into that table**]

### Lyris

There is a database called **Lyris** on the same server (vSQL01). &#x20;

The Lyris list server (on zLyris) uses the `Lyris` database on vSQL01. The recipients (members) and their send-time preferences are stored in the `members_` table. &#x20;

{% hint style="warning" %}
Both Lyris and the `Towing` DB maintain a list of subscribers.

1. The list in Lyris (the members list) is maintained and bad emails and unsubscribes them,&#x20;
2. The list in the `Towing` DB is not maintained and hence there are many unsubscribed members in to `Towing` DB email tables.

Recipients are selected by Lyris from its (curated) list, but the recipients preferences and streets etc are taken from the Email table in the `Towing` DB.
{% endhint %}

| Table Name | Important Fields                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Description                                                                                                 |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| members\_  | <p>EmailAddr_</p><p>FullName_</p><p>List_</p><p>Neighborhood</p><p>NowTowTimePreference</p><ul><li>Allston_Brighton_</li><li>BackBay_BeaconHill</li><li>BayVillage_</li><li>Charlestown_</li><li>Chinatown_Downtown_</li><li>Dorchester_</li><li>EastBoston_</li><li>Fenway_Kenmore_</li><li>HydePark_</li><li>JamaicaPlain_</li><li>Mattapan_</li><li>MidDorchester_</li><li>MissionHill_</li><li>NorthEnd_WestEnd_</li><li>Roslindale_</li><li>Roxbury_</li><li>SouthBoston_</li><li>SouthEnd_</li><li>WestRoxbury_</li></ul> | <p>A list of subscribed members.</p><p></p><p>[NowTowTimePreference] is 24hr clock for email time pref.</p> |

## Connected Services

### Lyris

Lyris is used for dispatching the street sweeping emails. This is an in-house email server (a mailing list server) which has a subscription API at [http://listserv.cityofboston.gov/subscribe/subscribe.tml](http://listserv.cityofboston.gov/subscribe/subscribe.tml).  The list subscribed to for sweeping alerts is `no-tow`.

{% hint style="info" %}
The Lyris server is available on `https://listserv.cityofboston.gov` login  can be provided by James Duffy and will be your city email address and a password.  If you are a Server Administrator you will be able to configure the actual Lyris service.
{% endhint %}

The list used is named `no-tow` , subscribers are added to the list, and removed from it when they unsubscribe.

Lyris is responsible for:

* Maintaining the list of current subscribers (members),and&#x20;
* Scheduling and initiating the 7am, 2pm and 5pm mailings each day, and
* Physically sending the emails required.

**BUT Lyris does not **

* Know the members preferred email time, or
* Know the members street selection, or
* Blackout dates, or
* Generate or manage the email body text

(These are managed by the :lyris.asp script)
