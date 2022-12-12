---
description: >-
  A No Tow subscription service where emails can be sent to registered
  constituents reminding them of sweeping days for a nominated street.
---

# Street Sweeping Reminders

{% hint style="success" %}
The Summer/Winter sweeping timetable is automatically adjusted by the scripts and does not require developer intervention to restrict notifications in the winter months.

See Summer/Winter section of lyris.asp below.
{% endhint %}

## Summary

Residents can use the page at [https://www.cityofboston.gov/publicworks/sweeping/](https://www.cityofboston.gov/publicworks/sweeping/).

First the resident searches for their street or a calendar.  Matching street results are returned from a master schedule/calendar and the resident nominates which streets should be included in the alert.  The resident then supplies an email address and a preferred email reminder time.

Subscription from this page registers the resident for street sweeping reminders, and also for Street Occupancy alerts.

The sending of emails is managed by a Lyris email server installed on the city network. The body of the email is constructed in a script called by lyris at `lyris.asp`.

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

**Note:**  _Lyris subscribes street occupancy and street sweeping members to the same **no-tow** list._

_**Note:** Lyris manages its list members and removes those which cannot be delivered to.  Hence the numbers in Lyris are lower than in the Towing DB tables._

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

If the default page is called with a querystring, then a search is performed and results returned. This is a relatively sophisticated search and provides schedules and the next sweeping event date.

```
?streetname=Portland+St
?neighborhood=xxxxx
?weekofmonth=xx&dayofmonth=xx
```

{% hint style="danger" %}
**Announcements column has a very outdated entry for 2013.**
{% endhint %}

### : subscribetostreet.asp

This page controls subscriptions and unsubscriptions.

_Subscriptions:_&#x20;

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

A list of cancellation and block-out dates is provided at the top of the script. (This list mirrors those input into default.asp).

The endpoint is designed to be called with an email address and date in the querystring. The script will return nothing if the email recipient is not scheduled to receive an email, and a full html if the recipient is due an email (according to the streets and Email time preference settings).

```
?email=david.upton@boston.gov&date=3/20/2021
```

The script runs SQL statements against the `PWDSweepingEmails`, `PWDSweeping` and `PwdDist` tables in the `Towing` database on vSQL01 and applies logic to determine if an email is required for that recipient.

(See Lyris below in Connected Services)

The script manages the annual program dates according to this "rule":

_The Daytime Street Cleaning Program runs from April 1 to November 30 in most Boston neighborhoods ... (however) ... Daytime street sweeping continues into the winter in the North End, South End, and Beacon Hill ... (which) ... stops on December 31 and starts up again on March 1._

{% hint style="success" %}
**The body text for the Street Sweeping reminder emails is set in this script.**
{% endhint %}

{% hint style="success" %}
**The body of the Street Occupancy emails is also set in this script.eh**
{% endhint %}

{% hint style="info" %}
**Email Schedule**

Emails are sent out by a scheduled mailing from lyris at 7am, 2pm and 5pm each day. The constituent can choose a time when they register. \
_A constituent may make multiple separate registrations for the same street but different notification times to get multiple notifications._

The script will accept a date as part of the querystring, but if no date is provided, then tomorrows date is assumed.

**7am, 2pm and 5pm Notifications:** _Lyris does not provide a date as part of its request_, so the script generates emails advising of daytime sweeping for following day, and overnight sweeping for the following night. \
e.g. Email generated on Friday provide notifications for daytime sweeping on Saturday, and overnight sweeping Saturday evening though Sunday morning.
{% endhint %}

{% hint style="info" %}
**Sweeping Cancellations**

The script can be manually edited and advance cancellation dates can be added. &#x20;

If the logic determines that a recipient should receive an email, and a matching date is found in the list of cancellations a note is added to the reminder advising that sweeping has in fact been cancelled.&#x20;

`dictCancellations.Add` All sweeping on this day is cancelled (i.e. both daytime and overnight). Content Editors should add to this dictionary (i.e. list of cancellations):\
1\. For planned city sweeping holidays (e.g. Christmas).  **Adding to this dictionary is a scheduled task to be undertaken every year at the start of November for the following calendar year.**\
****2. On notification from Public Works Dept when one-off, unplanned exceptional circumstances (e.g. snow storms) occur.&#x20;

`dictOvernightCancellations.Add` Only overnight sweeping is cancelled.  Content Editors should add to this dictionary (i.e. list of cancellations) when notified by Public Works Dept for one-off, unplanned exceptional circumstances (e.g. snow storms).\
_Note: In some cases cancellations are decided late in the evening, after emails have been sent._

* _Remember emails remind about sweeping the next day_&#x20;
* _**Because in most cases some emails reminders will have been sent before the cancellation occurs and the cancellation is added to the script - the city Content Editors also place a cancellation notice on the homepage of boston.gov.**_
{% endhint %}

{% hint style="info" %}
**Summer/Winter Adjustments.**

The City has 3 classifications which indicate which winter sweeping program is operated on city managed streets. Streets are classified as _**Normal**_ (no sweeping 01 Dec through 31 March), _**Northend Pilot**_ (no sweeping 01 Jan through last day of Feb) and **YearRound** (streets sweept all year). .

The `PwdSweeping`table in the `Towing` database holds information on streets managed by the city and columns `yearround` and `northendpilot` to indicate the streets sweeping program. (if neither column has a "1" in it, then the street is "Normal"). There also appears to be a **Charlestown Pilot** (defined as streets in district 1C) The timetable is  controlled using the same flags, but a note is added to emails to advise elevated fines in lieu of towing.

If there is an issue with a constituent receiving a sweeping notification during a winter "no-sweeping" period then:

1. Ask the constituent to check signage to be sure the mail was sent in error.\
   If the mail does appear to be incorrect, then
2. Check the Street definition at the page:\
   &#x20;     cityofboston.gov/publicworks/sweeping/admin/default.aspx \
   (check dashlane for the password)\
   Ensure the checkboxes next to "Winter Extension" (aka Northend Pilot) and "Year Round" are set properly,\
   The checkboxes are correctly set, then
3. &#x20;Contact a developer.
{% endhint %}

### :admin/Default.aspx

This page allows an administrator to login, alter and export detailed and low-level street cleaning metadata in the `PwdSweeping` table.

Login information is in DashLane.

{% hint style="info" %}
The `PwdSweeping` table, contains all information on the partitioning and scheduling of Boston City streets cleaning activities.

[See notes below.](street-sweeping-reminders.md#undefined)
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
| PwdSweeping       | <ul><li>MainID</li><li>St_name</li><li>Dist(rict)</li><li>StartTime</li><li>EndTime</li><li>Side</li><li>...<br>[schedule info]</li></ul> | <p>Contains street sweeping schedule information for streets in the city.</p><p></p><p>This table is managed from the admin/default.aspx page.</p>                                                                                                                              |
| PwdSweepingEmails | <ul><li>EmailAddr</li><li>StreetID</li></ul>                                                                                              | <p>Contains information on who has subscribed to what.</p><p><br>StreetID maps to the MainID in the PwdSweeping table.<br></p><p>This table is maintained by the scripts in this sub-service.  It is also used by the <strong>Subscription Search</strong> (aka remind me).</p> |

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

**BUT Lyris does not**&#x20;

* Know the members preferred email time, or
* Know the members street selection, or
* Blackout dates, or
* Generate or manage the email body text

(These are managed by the :lyris.asp script)

### Street Administration

A page at admin/default.aspx allows an administrator to maintain information on the partitioning and scheduling of Boston City streets cleaning activities used (primarily) by the NoTow suite of apps..

PWD tend to re-organize street cleaning at the start of the "season" in March/April, and they will update the database table, using the admin page, at that time.  Subsequent schedule changes are maintained in the table by PWD staff as they occur.  The main PWD staff member doing this work is Paul Taylor.  Any errors and omissions reported during the year are reviewed by Paul Taylor and (if required) he updates/alters the database using the admin page.  This final step could be done by Digital provided a note is sent to Paul after the change is made.

The current street sweeping database can be dumped to a csv here: [https://www.cityofboston.gov/publicworks/sweeping/admin/database.csv](https://www.cityofboston.gov/publicworks/sweeping/admin/database.csv)

{% hint style="info" %}
The `PwdSweeping` is not a copy of any other database, and it does not appear that there is any other centralized record of the street partitioning and sweeping schedule.
{% endhint %}

{% hint style="success" %}
Districts can be found from an ArcGIS map on the COB ArcGIS server.

[http://zppwdapp01.web.cob:83/apps/stations/](http://zppwdapp01.web.cob:83/apps/stations/)
{% endhint %}

{% hint style="success" %}
Additional street information, including linear feet from street origin to intersections can be found here: [http://zppwdapp01.web.cob:83/apps/stations/](http://zppwdapp01.web.cob:83/apps/stations/) (created and maintained by Paul Taylor)
{% endhint %}

{% hint style="danger" %}
**Potential Data Issues**

_**(Needs verifying)**_

The email registration for Street Occupancy and Street Sweeping alerts uses an ID from the the PwdSweeping table (MainID). If a street is split or merged, it is possible that email registrations could become orphaned.

Examples:

1. 169th Street get split into two (for sweeping activities).  The original schedule started at 0ft and ended at 1000ft, with a MainID of 236 and sweeping was even days. The street now gets split by adding a new street partition (MainID of 423) from 501ft to 1000ft  and sweeping is set for odd days. The original (MainID 236) is updated so it  now starts at 0ft but ends at 500ft.  The issue is that anyone who lives between 500 and 1000ft will be registered against MainID 236 instead of MainID 423 and will thus get email reminders for the wrong days.
2. A similar type of issue could occur if streets are merged, but in this case registrations the street being "removed" would stop getting emails even though from their perspective the sweeping continues unchanged.
{% endhint %}
