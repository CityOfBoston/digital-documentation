---
description: >-
  A No Tow subscription service where city residents can register a license
  plate and then receive alerts (email/text/voice) if a vehicle with that plate
  is towed.
---

# Towing Alerts

## Summary

Residents can use the page at [https://www.cityofboston.gov/towing/alerts](https://www.cityofboston.gov/towing/alerts).

The resident is able to register a license plate and receive one or any of:

* an email alert (managed by SQL Server)
* a text alert (SMS via email)
* a voice alert (managed by Twilio)

Every 15 minutes the city gets [an update from the police](towing-alerts.md#police-updates) on towed vehicles.  For each new vehicle towed the license plate is checked against plates registered by residents and alerts are sent when matches are found.&#x20;

### **Statistics**

| Statistics as at 10/27/2021 | Value      |
| --------------------------- | ---------- |
| Email subscribers List      | 765,168    |
| Voice Subscribers           | 3.292      |
| SMS Subscribers             | 1 (Satyen) |

## Code

This is an ASP application hosted on **ZPCOBWEB01.web.cob** (a DMZ IIS Server).

The code is is found in the following folder:

```
D:\wwwcob\towing\alerts
```

### : default.asp

This page generates forms to collect information for email, text and voice alert subscriptions.  The forms all submit to `subscribe.asp` which does the actual subscription for alerts that are sent when a registered vehicle is towed.

### : subscribe.asp

**Email:**

_Subscription -_ First the script checks if the email & plate is already subscribed. If it is not, then the script checks how many license plates are already registered against the email address, if its more than 10 it wont register this new one (unless the email is on a whitelist - see box out below).  If the plate is not registered to this address, and the address has less than 10 plates registered to it (or is whitelisted) then the email/plate combo is registered in the table `towed_emails` in the database `Towing` on vSQL01 (aka ZPDMZSQL01).

_Unsubscription_ - First the script checks if the email & plate is already subscribed. If it is, then the entry is removed from `towed_emails` and is added to `towed_emails_optout` in the database `Towing` on vSQL01 (aka ZPDMZSQL01).

{% hint style="info" %}
**Fleet Owners Whitelist.**

Line 125 of subscribe.asp contains a white list of subscribers who can register more than 10 vehicles.
{% endhint %}

**Text:**

Text subscription is completed by adding a record to the `towing_emails` table, but using an email address that routes through the subscribers telephone provider.  In this way sms messages can be sent without needing to use an sms gateway.

{% hint style="danger" %}
**There is a "No Provider' option, which should ideally be removed.**

**Provider email to text gateways can be found here:** [**https://avtech.com/articles/138/list-of-email-to-sms-addresses/**](https://avtech.com/articles/138/list-of-email-to-sms-addresses/)
{% endhint %}

**Note:** We do have the option to send SMS via Twilio using a stored procedure in the `twiSQL` database, however this incurs an additional cost.  As at 2021-11-17, this process does work, and is used by the SMS block in the `sp_process_towing_messages` stored procedure.  To be certain that SMS messages are delivered, it could be that users who select 'No Provider' are added to the `towing_sms` table.

**Voice:**

_Subscription -_ First the script checks if the email & plate is already subscribed. If it is not, then the script checks how many license plates are already registered against the email address, if its more than 10 it wont register this new one (there is no whitelist for voice registrations).  If the plate is not registered to this address, and the address has less than 10 plates registered to it then the email/plate combo is registered in the table `towed_phonenumbers` in the database `Towing` on vSQL01 (aka ZPDMZSQL01).

_Unsubscription_ - First the script checks if the email & plate is already subscribed. If it is, then the entry is removed from `towed_phonenumbers` and is added to `towed_phonenumbers_optout` in the database `Towing` on vSQL01 (aka ZPDMZSQL01).

### : remindme.asp

This page allows the user to provide an email address and the system will email a list of plates registered to that email address.

The email handler used is installed as part of the IIS service at ZPCOBWEB01.web.cob (10.241.250.22), and the SMTP server the mail is routed through is at **smtp.web.cob** (10.241.250.209)

## Database

{% hint style="info" %}
The utility code which manages connections to the Database Server and posts queries etc to the server is contained within:

`D:\wwwcob\includes\stdf.inc`

**Note:** the same include file has a mail handler which connects to (public) mail.cityofboston.gov (140.241.251.209) - but this seems to be deprecated.
{% endhint %}

The database server used by this sub-service is **vSQL01** (aka ZPDMZSQL01).  The server is hosted in the DMZ in the web.cob (aka lincdom) domain.  Developers need a separate and specific account to be set up on the web.cob domain to view/edit databases and tables on this server.

### Towing

The primary database used by this sub-service is **Towing**. This database holds information on registered plates and vehicles towed.

The tables used by this sub-service are:

| TableName                                                                | Key Fields                                                                                                                         | Description                                                                          |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| <p>towed_emails</p><p>towed_phonenumbers</p><p>towed_sms</p>             | <ul><li><p>subscriber_email, or </p><ul><li>subscriber_phone</li></ul></li><li>subscriber_plate</li><li>subscriber_state</li></ul> | Contains list of mail, sms and voice subscribers and their plates to monitor.        |
| Towline\_bpd                                                             | <ul><li>License Plate</li><li>Tow Date-Time</li><li>Infringement Location</li></ul>                                                | Contains a list of all vehicles towed by services authorized by the City.            |
| <p>towed_emails_log</p><p>towed_phonenumbers_log</p><p>towed_sms_log</p> |                                                                                                                                    | Contains log of alerts raised                                                        |
| towed\_import\_log                                                       |                                                                                                                                    | Contains a summary of activity (used in daily report)                                |
| towed\_phone\_alerts\_queue                                              |                                                                                                                                    | Contains a queue for voice messages which are then processed by an external service. |
| Towline\_complete                                                        |                                                                                                                                    | Archive for the complete list of towed vehicles since time began.                    |

**The email/voice is handled by the SMTP service on the MSSQL Server.**

### _twiSQL_

Manages SMS message dispatch via the Twilio SMS gateway.

This database is used by the `sp_process_towing_messages`.  There are no tables inside the database, and 3 stored procedures.  There is also an Assembly (twilioSQL) loaded., plus (possibly auto-created) a Service Broker.

The stored procedure `SendBrokerMessage` is called in order to route an SMS message via Twilio.

## Connected Services

### Police Updates

The police update information on newly towed vehicles every 15 minutes.  The police have a job/process that pushes the data to vSQL01.&#x20;

![](<../../../.gitbook/assets/image (29).png>)

The police data is inserted directly into the `Towline_bpd`table by the Police IT department (contacts below). The actual SSIS-ODBC process involves truncating the towline\_bpd table and then bulk inserting a complete set of new records. &#x20;

**Note:** The MSSQL bulk insert does not fire triggers by default.

#### **Permissions**

The police account (youvebeentowed (?)) needs permission to truncate the `Towline_bpd` table and to (bulk) insert new records.  It does not need permissions to stored procedures, triggers or the twiSQL database.

#### **Contacts**

<table><thead><tr><th width="204.11859838274933">Staff</th><th>Position</th><th>Email</th></tr></thead><tbody><tr><td>Rich Petruccelli</td><td>Data Services Manager</td><td><a href="mailto:Rich.Petruccelli@pd.boston.gov">Rich.Petruccelli@pd.boston.gov</a></td></tr><tr><td>Thomas Hutchings (Hutch)</td><td></td><td><strong>t</strong><a href="mailto:thomas.hutchings@pd.boston.gov">homas.hutchings@pd.boston.gov</a></td></tr><tr><td>Frank Alexopoulos</td><td>Apps development</td><td><a href="mailto:Frank.Alexopoulos@pd.boston.gov">Frank.Alexopoulos@pd.boston.gov</a></td></tr></tbody></table>

### Outbound Communication (email, sms and voice)

#### **Reminder emails**

Reminder emails created and sent by the `remindme.asp` page (one-time & on-demand by the resident) are routed through an SMTP server the mail at **smtp.web.cob.**

#### Alert Initiation

As the police update the `towline_bpd` table (i.e. as vehicles are towed) alerts are originated and handled by the **Towing** database and **TwiSQL** database on the MSSQL Server at **vSQL01.** &#x20;

Alerting is fully decoupled from the cityofboston.gov hosted asp pages, and initiated by the MSSQL server vSQL01.

**There is a stored procedure, `sp_process_towing_messages` , in the `Towing` database which is executed every 5 minutes by the  SQL Job; `TowingSendMail`.** &#x20;

`sp_process_towing_messages` reads the `towline_bpd` table and discovers new tow events. It then determines which tows have subscriptions.  Finally, it dispatches the outbound communications to subscribers.

* The sp evaluates the inserted rows, looks to see if the license plate is registered (`towed_emails, towed_phonenumbers and towed_sms`), and if so ends out an alert. &#x20;
* The sp uses the system stored procedure `sp_send_dbmail`, to send emails directly to the subscriber from the MS SQL server. \
  Some SMS subscribers are also in the `towed_emails` table because the subscriber has registered for SMS/text by providing a phone number and provider. (see box below)
* The sp interfaces directly with Twilio (for unspecified-provider SMS's)&#x20;
* The sp drops records into a queue for voice processing (which runs on a scheduled task). &#x20;
* The sp records which recipients have been communicated with, and which have been processed
* The sp maintains statistics on what has been sent out.

**Alert emails:**

Emails are sent directly from `Towing.dbo.sp_process_towing_messages` . The sp constructs and send them out using the built-in MSSQL email service (from `msdb.dbo.sp_send_dbmail`).

{% hint style="info" %}
The email process handles both emails and (the majority of) sms messages. SMS strategy is to send an email to a mobile provider specific email address which then routes the email as an SMS to the recipient/subscriber.

For example the email record in `towed_emails` **781###@att.net** was registered as phone number 781### and AT\&T as the provider. Using that email format prompts an email-to-sms service at att.net.
{% endhint %}

**SMS/Text messages:**

The majority of text messages are sent via email-to-sms (see box-out above).

**Note:** Some subscriptions are saved in the table `towed_sms`and initiated by `process_towing_messages` .

**Voice calls:**

* The process is managed within `sp_process_towing_messages` which is executed every 5 minutes by the SQLAgent Job `TowingSendMail`. &#x20;
* Voice calls identified in the stored procedure are queued into the table `Towing_twilio_Queue` in the `Towing` database.
* Every 10 minutes, a scheduled task `Towing_Twilio_Queue` runs on `zpcobweb10` (10.241.250.22). The task runs a script `c:\installs\scripts\curl_towing_queue_process.bat` .
* The script simply calls `twilio-alert-place.aspx.` \
  `curl.exe -connect-timeout 300 http://zpcobweb01.web.cob/towing/alerts/twilio/twilio-alert-place.aspx`
* `Twilio-alert-place.aspx` app/script calls a stored procedure `Towing.dbo.spTowingTwilioQueueRead` which fetches queued voice messages from the `towed_phone_alerts_queued` table.  For each queued message the endpoint payload and creates a Twilio object and initiates the outbound call (via the Twilio REST API). The app/script also removes the record from the queue (`towed_phone_alerts_queued`) using the stored procedure `towing.dbo.spTowingTwilioQueueDelete`.

{% hint style="info" %}
The message which is sent to theTwilio REST service (to make the voice call to the subscriber) includes a call to an endpoint `twiml-alert-content.aspx`  This app/script receives the towed vehicle details from the querystring/payload and creates and returns the correct message for the Twilio voice service to read out.

The message also defines an interaction where the call recipient can hear the message again, obtain tow company information or unsubscribe (using `spTowingTwilioUnsubscribe`) from future voice alerts.&#x20;
{% endhint %}

{% hint style="info" %}
There is an additional endpoint at `Twilio-incoming.aspx` . This is provided to handle incoming calls to the No-Tow telephone number.  This app/script determines the caller phonenumber and looks up recent towing records to see if a vehicle with a plate registered to this number has been towed.  If so the details of the tow event are read back to the caller
{% endhint %}

#### Permissions&#x20;

The job which initiates the sp which processes new alerts does need extensive permissions.

**Stored Procedures** (execute permission)**:**

* **msdb.dbo.sp\_send\_dbmail:** to actually send out emails using MSSQL Mail services.
* _<mark style="color:purple;">**twiSQl.dbo.SendBrokerMessage**</mark><mark style="color:purple;">: This interacts with a service on the twiSQL DB on the same server (????) There is a twilioSQL assembly loaded on the twiSQL database, but not on the Towing DB.</mark>_

**Functions** (Call/Execute permission)**:**

* **Towing.dbo.functionVerboseReasonCode**
* **Towing.dbo.functionUnabbreviateTowCompany**
* **Towing.dbo.functionUnabbreviateYear**
* **Towing.dbo.functionUnabbreviateCarMake**
* **Towing.dbo.functionUnabbreviateCarColor**
* **Towing.dbo.URLEncode**

**Tables** (Insert and read permission)**:**

* **Towing.dbo.Towline\_bpd** _(also requires delete permission)_
* **Towing.dbo.towed\_import\_log**
* **Towing.dbo.towed\_emails**
* **Towing.dbo.towed\_emails\_optout**
* **Towing.dbo.towed\_alerts\_log**
* **Towing.dbo.towed\_phonenumber**
* **Towing.dbo.towed\_phone\_alerts\_queued**
* **Towing.dbo.towed\_phone\_alerts\_log**
* **Towing.dbo.towed\_sms**
* **Towing.dbo.towed\_sms\_alerts\_log**

### Twilio - Voice Alerts

Voice alerts are somehow processed by Twilio using a complicated call-back process to cityofboston.gov. &#x20;

**This needs further investigation and access to the twilio UI.**
