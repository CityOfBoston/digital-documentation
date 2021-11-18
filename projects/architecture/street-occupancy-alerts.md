---
description: >-
  A No Tow subscription service where emails can be sent to registered residents
  advising them of temporary parking changes (no parking permits issued  etc)
  for a nominated street.
---

# Street Occupancy Alerts

## Summary

Residents can use the page at [https://www.cityofboston.gov/streetoccupancy/search/](https://www.cityofboston.gov/streetoccupancy/search/).

First the resident searches for their street or a specific permit number.  Matching permits are returned as a list, along with a list of streets matching the search term or permit provided.  The resident nominates which streets should be included in the alert and then supplies an email address and a preferred email reminder time.

{% hint style="info" %}
Email body text is generated in the `lyris.asp` script and the email generation is managed by the Lyris server (listserv.boston.gov)
{% endhint %}

### Statistics

_Lyris subscribes street occupancy members to the no-tow list._

| Statistics as at 10/27/2021 | Value                                |
| --------------------------- | ------------------------------------ |
| Total List                  | 10,559 (OccupancyEmails - Towing DB) |
|                             |                                      |

## Code

This is an ASP application hosted on **ZPCOBWEB01.web.cob** (a DMZ IIS Server).

The code is found in the following folder:

```
D:\wwwcob\streetoccupancy\search\
```

### : default.asp

The search form for the registration process is contained on this page.  The search is conducted against the `occupancyupdates, SAM_streets` and `OccupancyPermitsHansen` tables in the `Towing`database on vSQL01 (aka ZPDMZSQL01).

The residents subscription choices (streets & dates) are managed by this page for use in the subscription process.

Subscription is initiated on this page but the actual subscription process is handed off to `subscribetostreet.asp` ([see below](street-sweeping-reminders.md#subscribetostreet-asp)).

### : subscribetostreet.asp

This page controls subscriptions and unsubscriptions.

_Subscriptions: _

Subscribing involves first removing and then adding the residents email and street into the table `OccupancyEmails` in the `Towing` database on vSQL01 (aka ZPDMZSQL01).

* if this is a first time subscription, the user is added or removed from the Lyris email server at [http://listserv.cityofboston.gov/subscribe/subscribe.tml](http://listserv.cityofboston.gov/subscribe/subscribe.tml).
* If this is not a first time subscription, then the users time preference is added to the `members_` table in the `Lyris` database on vSQL01 (aka(ZPDMZSQL01).

_Unsubscriptions:_

Unsubscribing involves removing the residents email and street from the table `OccupancyEmails` in the `Towing` database on vSQL01 (aka ZPDMZSQL01).

## Database

{% hint style="info" %}
The utility code which manages connections to the Database Server and posts queries etc to the server is contained within:

`D:\wwwcob\includes\stdf.inc`
{% endhint %}

The database server used by this sub-service is **vSQL01** (aka ZPDMZSQL01).  The server is hosted in the DMZ in the web.cob (aka lincdom) domain.  Developers need a separate and specific account to be set up on the web.cob domain to view/edit databases and tables on this server.

### Towing

The database used by this sub-service is **Towing.**

It appears that the code uses a trusted connection to the database server. To connect to this database you first need to have an account in the lincdom domain.  Then you need to have the Microsoft SQL Server Management Studio installed on your work PC. Then you need to connect to the server at **zpdmzsql01.web.cob** using your lincdom account (user:LINCDOM\username + password:userpassword).  This should work.

The tables used by this sub-service are:

* `OccupancyUpdates`appears to contain information on that last date the permits were synchronized from some external system (probably Hansen),
* `OccupancyPermits/OccupancyPermitsHansen`contains information on issued permits,
* `SAM_Streets` contains street name information, from the (most likely sync'd from the SAM system).
* `OccupancyEmails` contains a list of subscribers for street occupancy emails
* `members_`table contains information on email scheduling.

### Lyris

There is a database called **Lyris** on the same server (vSQL01). &#x20;

It seems that the Lyris mail server uses the `Lyris` database on vSQL01. It also seems the recipients and their send-time preferences are described in the `members_` table.  Further, Lyris seems to get the streets the members are subscribed to from the `OccupancyEmails` table.&#x20;

&#x20;The list used is `no-tow`.

{% hint style="danger" %}
**Get a login to Lyris to see how the listserv process works.**
{% endhint %}

{% hint style="danger" %}
**It is possible that the emails are generated outside of Lyris and by the MSSQL Server.  This needs investigation.**
{% endhint %}

## Connected Services

### SAM

Information from the CoB custom application **Street Address Management (SAM)** appears to be used to provide some street information.

It is likely that there is some ERL process (managed by Analytics?) to move data from SAM databases (MSSQL) to the Towing Database on ZPDMZSQL01 (vSQL01).

{% hint style="info" %}
Someone should find this migration process.  Maybe Sundar/Satyen or Analytics have some knowledge.
{% endhint %}

### Hansen

Hansen is the CoB permitting system.

Hansen is the authoritative source for issued permits. It is likely that there is some ERL process (managed by Analytics?) to move data from Hansen (MSSQL) to the Towing Database on ZPDMZSQL01 (vSQL01).  The date of the last data sync with Hansen is shown on the search page.

### Lyris

Lyris is used for handling the street occupancy emails. This is an in-house email server (a mailing list server) which has an API at [http://listserv.cityofboston.gov/subscribe/subscribe.tml](http://listserv.cityofboston.gov/subscribe/subscribe.tml).  The list used is `no-tow`.
