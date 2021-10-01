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

## Code

This is an ASP application hosted on **ZPCOBWEB01.web.cob** \(a DMZ IIS Server\).

The code is is found in the following folder:

```text
D:\wwwcob\publicworks\sweeping
```

### : default.asp

This page contains a list of cancellations.  It looks like known sweeping holidays \(e.g. Veterans day\) are loaded for the year in advance, and then ad-hoc cancellations \(e.g. Snow or Ice\) are loaded as the cancellation is advised by PWD.  Cancellations for general street sweeping and overnight sweeping are maintained separately.

The search form for the registration process is contained on this page.  The search is conducted against the `PwdSweeping` table in the `Towing` database on vSQL01 \(aka ZPDMZSQL01\).

The residents subscription choices \(streets & dates\) are managed by this page for use in the subscription process.

Subscription is initiated on this page but the actual subscription process is handed off to subscribetostreet.asp \(see below\).

Also the user can download a file which can be imported into calendars supporting the iCalendar format \(this is most calendars\). The file generation and download is managed by addtocalendar.ics.asp \(see below\)

{% hint style="danger" %}
**Announcements column has a very outdated entry for 2013.**
{% endhint %}

### : subscribetostreet.asp

This page controls subscriptions and unsubscriptions.

Subscriptions/unsubscriptions involve adding or removing the residents email and street into the table `PwdSweepingEmails` in the `Towing` database on vSQL01 \(aka ZPDMZSQL01\).

Also, the subscription is added or removed from the Lyris email server at [http://listserv.cityofboston.gov/subscribe/subscribe.tml](http://listserv.cityofboston.gov/subscribe/subscribe.tml).

### : addtocalendar.ics.asp

_The iCalendar \(ics\) format is presently supported by: Google Calendar, Apple Calendar \(formerly iCal\), IBM Notes \(formerly Lotus Notes\), Yahoo! Calendar, Evolution \(software\), eM Client, Lightning extension for Mozilla Thunderbird and SeaMonkey, and partially by Microsoft Outlook and Novell GroupWise_

This page extracts a schedule from the `PwdSweeping` table in the `Towing` database on vSQL01 \(aka ZPDMZSQL01\) and formats into an ics format and then downloads the ics file to the residents computer.

## Databases

The database server used by this sub-service is **vSQL01** \(aka ZPDMZSQL01\).  The server is hosted in the DMZ in the web.cob \(aka lincdom\) domain.  Developers need a separate and specific account to be set up on the web.cob domain to view/edit databases and tables on this server.

The database used by this sub-service is **Towing.**

It appears that the code uses a trusted connection to the database server. To connect to this database you first need to have an account in the lincdom domain.  Then you need to have the Microsoft SQL Server Management Studio installed on your work PC. Then you need to connect to the server at **zpdmzsql01.web.cob** using your lincdom account \(user:LINCDOM\username + password:userpassword\).  This should work.

The tables used by this sub-service are:

<table>
  <thead>
    <tr>
      <th style="text-align:left">TableName</th>
      <th style="text-align:left">Key Fields</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">PwdSweeping</td>
      <td style="text-align:left">
        <ul>
          <li>MainID</li>
          <li>St_name</li>
          <li>Dist(rict)</li>
          <li>StartTime</li>
          <li>EndTime</li>
          <li>Side</li>
          <li>...
            <br />[schedule info]</li>
        </ul>
      </td>
      <td style="text-align:left">Contains street sweeping schedule information for streets in the city.</td>
    </tr>
    <tr>
      <td style="text-align:left">PwdSweepingEmails</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

PwdSweeping is maintained elsewhere and is active - The last update \(as at 2021-10-01\) was 2021-09-07.  \[**Checking with Sundar on how the data gets into that table**\]

## Connected Services

Lyris is used for emails.

Twillio is used for voice and text alerts.

