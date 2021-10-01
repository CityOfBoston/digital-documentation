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

**Email and text:**

_Subscription -_ First the script checks if the email & plate is already subscribed. If it is not, then the script checks how many license plates are already registered against the email address, if its more than 10 it wont register this new one \(unless the email is on a whitelist - see box out below\).  If the plate is not registered to this address, and the address has less than 10 plates registered to it \(or is whitelisted\) then the email/plate combo is registered in the table `towed_emails` in the database `Towing` on vSQL01 \(aka ZPDMZSQL01\).

_Unsubscription_ - First the script checks if the email & plate is already subscribed. If it is, then the entry is removed from `towed_emails` and is added to `towed_emails_optout` in the database `Towing` on vSQL01 \(aka ZPDMZSQL01\).

{% hint style="info" %}
Line 125 contains a white list of subscribers who can register more than 10 vehicles.
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

## Connected Services

### Emails originated by sub-service

The emails created and sent by the `remindme.asp` page are routed through an SMTP server the mail at **smtp.web.cob.**

## Offline Services

### Police Updates

### Alert Generation

qwqw

