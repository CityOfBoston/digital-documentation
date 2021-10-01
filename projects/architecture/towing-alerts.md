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

### : remindme.asp

## Database

## Connected Services

## Offline Services

### Police Updates

### Alert Generation

qwqw

