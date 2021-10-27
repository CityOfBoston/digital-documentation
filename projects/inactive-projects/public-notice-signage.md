---
description: >-
  Large digital screen on the first floor of Boston City Hall that displays
  public notices
---

# Public Notice Signage

We support a web app that displays public notices for upcoming meetings on a TV on the 1st floor of City Hall.

[https://apps.boston.gov/public-notices/](https://apps.boston.gov/public-notices/)

The data for this is fetched client-side from a JSON API on Boston.gov.

#### Contacts

* Product Management (at one point): Reilly Zlab
* API Engineering: David Upton
* Design: Sebastian Ebarb

_Historical contacts:_

* Front-End Engineering: Fiona Hopkins

#### History

This app was previously written with Vue and hosted on Heroku. See [https://github.com/CityOfBoston/notice-signage](https://github.com/cityofboston/digital/wiki/CityOfBoston/notice-signage). It used [Pusher](https://www.pusher.com) to control when it reloaded data or refreshed its code.

In January 2019 we reimplemented it as a React/Next.js app in the Digital monorepo: [https://github.com/CityOfBoston/digital/tree/develop/services-js/public-notices](https://github.com/CityOfBoston/digital/tree/develop/services-js/public-notices) The new code fetches by itself on a 5 minute loop (with retries / exponential backoff) and automatically reloads itself when the server code changes.

It is now served as static files from S3.

_Case study on the original project implementation: _[_https://www.boston.gov/departments/digital-team/digital-team-case-studies-web-development_](https://www.boston.gov/departments/digital-team/digital-team-case-studies-web-development)__

#### BrightSign

The app is shown on a BrightSign HD222 on the 1st floor. It’s running a presentation that consists of just one HTML5 slide pointing at the above URL.

Updates to the sign can be made by overwriting its presentation using BrightSign Author. Talk to the Digital team for the IP address, or ask NOC, since it’s reserved.
