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

* an email alert \(managed by SQL Server\)
* a text alert \(managed by Twillio\)
* a voice alert \(managed by Twillio\)

Every 10 minutes the city gets [an update from the police](towing-alerts.md#police-updates) on towed vehicles.  For each new vehicle towed the license plate is checked against plates registered by residents and alerts are sent when matches are found.  See [Offline processes](towing-alerts.md#alert-generation) for more information.

## Code

This is an ASP application hosted on **ZPCOBWEB01.web.cob** \(a DMZ IIS Server\).

The code is is found in the following folder:

```text
D:\wwwcob\towing\alerts
```

### : default.asp

This page generates forms to collect information for email, text and voice alert subscriptions.  The forms all submit to `subscribe.asp` which does the actual subscription for alerts that are sent when a registered vehicle is towed.

### : subscribe.asp

**Email:**

_Subscription -_ First the script checks if the email & plate is already subscribed. If it is not, then the script checks how many license plates are already registered against the email address, if its more than 10 it wont register this new one \(unless the email is on a whitelist - see box out below\).  If the plate is not registered to this address, and the address has less than 10 plates registered to it \(or is whitelisted\) then the email/plate combo is registered in the table `towed_emails` in the database `Towing` on vSQL01 \(aka ZPDMZSQL01\).

_Unsubscription_ - First the script checks if the email & plate is already subscribed. If it is, then the entry is removed from `towed_emails` and is added to `towed_emails_optout` in the database `Towing` on vSQL01 \(aka ZPDMZSQL01\).

{% hint style="info" %}
Line 125 contains a white list of subscribers who can register more than 10 vehicles.
{% endhint %}

**Text:**

{% hint style="danger" %}
**It seems that you cannot register an sms/text alert, but the process may appear to have done so successfully.**
{% endhint %}

**Voice:**

_Subscription -_ First the script checks if the email & plate is already subscribed. If it is not, then the script checks how many license plates are already registered against the email address, if its more than 10 it wont register this new one \(there is no whitelist for voice registrations\).  If the plate is not registered to this address, and the address has less than 10 plates registered to it then the email/plate combo is registered in the table `towed_phonenumbers` in the database `Towing` on vSQL01 \(aka ZPDMZSQL01\).

_Unsubscription_ - First the script checks if the email & plate is already subscribed. If it is, then the entry is removed from `towed_phonenumbers` and is added to `towed_phonenumbers_optout` in the database `Towing` on vSQL01 \(aka ZPDMZSQL01\).

### : remindme.asp

This page allows the user to provide an email address and the system will email a list of plates registered to that email address.

The email handler used is installed as part of the IIS service at ZPCOBWEB01.web.cob \(10.241.250.22\), and the SMTP server the mail is routed through is at **smtp.web.cob** \(10.241.250.209\)

## Database

{% hint style="info" %}
The utility code which manages connections to the Database Server and posts queries etc to the server is contained within:

`D:\wwwcob\includes\stdf.inc`

**Note:** the same include file has a mail handler which connects to \(public\) mail.cityofboston.gov \(140.241.251.209\) - but this seems to be deprecated.
{% endhint %}

The database server used by this sub-service is **vSQL01** \(aka ZPDMZSQL01\).  The server is hosted in the DMZ in the web.cob \(aka lincdom\) domain.  Developers need a separate and specific account to be set up on the web.cob domain to view/edit databases and tables on this server.

### Towing

The primary database used by this sub-service is **Towing**. This database holds information on registered plates and vehicles towed.

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
      <td style="text-align:left">
        <p>towed_emails</p>
        <p>towed_phonenumbers</p>
        <p>towed_sms</p>
      </td>
      <td style="text-align:left">
        <ul>
          <li>subscriber_email, or
            <ul>
              <li>subscriber_phone</li>
            </ul>
          </li>
          <li>subscriber_plate</li>
          <li>subscriber_state</li>
        </ul>
      </td>
      <td style="text-align:left">Contains list of mail and voice subscribers and their plates to monitor.</td>
    </tr>
    <tr>
      <td style="text-align:left">Towed_Cars</td>
      <td style="text-align:left">
        <ul>
          <li>License Plate</li>
          <li>Tow Date-Time</li>
          <li>Infringement Location</li>
        </ul>
      </td>
      <td style="text-align:left">Contains a list of all vehicles towed by services authorized by the City.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>towed_emails_log</p>
        <p>towed_phonenumbers_log</p>
        <p>towed_sms_log</p>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Contains some sort of log of alerts raised</td>
    </tr>
    <tr>
      <td style="text-align:left">towed_warning_log</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Contains some soft of warnings... possible internal issue log?</td>
    </tr>
  </tbody>
</table>

A _trigger_ on the `Towed_Cars`table causes a stored procedure to run when data is added to the table.  The stored procedure evaluates the inserted rows, looks to see if the license plate is registered \(`towed_emails, towed_phoennumbers and towed_sms`\), and if so ends out an alert.  

**The email/voice is handled by the SMTP service on the MSSQL Server.**

## Connected Services

### Emails originated by sub-service

Reminder emails created and sent by the `remindme.asp` page \(one-time & on-demand by the resident\) are routed through an SMTP server the mail at **smtp.web.cob.**

Alert emails which are generated as vehicles are towed are originated and handled by the **Towing** database on the MSSQL Server at **vSQL01.** 

### Police Updates

The police update information on newly towed vehicles every 10 minutes.  It is believed that this information initially goes into the mainframe, and then an ETL process pushes the data to vSQL01. 

{% hint style="info" %}
It is possible that the police push directly to vSQL01.
{% endhint %}

### Twillio - Voice Alerts

### Twillio - Text Alerts

qwqw

