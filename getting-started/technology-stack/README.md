# Technology stack and technologies used

## Main City Website (boston.gov)

[![Build Status](https://camo.githubusercontent.com/059573f47d24bb422b9c5436d9da728195188774/68747470733a2f2f7472617669732d63692e6f72672f436974794f66426f73746f6e2f626f73746f6e2e676f762d64382e706e67)](https://travis-ci.org/CityOfBoston/boston.gov-d8)[\
​](https://travis-ci.org/CityOfBoston/boston.gov-d8)The main City of Boston website is a CMS developed using Drupal (a PHP - MySQL framework) hosted on Linux - Apache (LAMP) in the Cloud. Implemented as Drupal 7 in 2016, migrated to Drupal 8 in 2019, then Drupal 9 in March 2022.

{% content-ref url="../../guides/drupal-8/" %}
[drupal-8](../../guides/drupal-8/)
{% endcontent-ref %}

## Cityofboston.gov

Legacy website and applications (pre-Drupal) hosted 'on-prem' at the City. Nearly all of these items live on zpcobweb01 with sql databases -- mainly ZPDMZSQL01 and ZPCOBSQL22. Most of these are .asp or .aspx files.

## Web Applications

Our web app stack includes React, NextJS, Node.js/Hapi, and you can check out a more complete list by following the link below.

Some of these are attached to vsql22 for data.

{% content-ref url="web-applications.md" %}
[web-applications.md](web-applications.md)
{% endcontent-ref %}

## Third-party integrations and tools

| Tool                                                                    | Purpose                                                                                                                                           |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| AgilePoint                                                              | Form generator/manager                                                                                                                            |
| [AWS](https://docs.aws.amazon.com)                                      | App hosting                                                                                                                                       |
| BoldChat                                                                | Real live chat platform used by 311; paid for by DoIT                                                                                             |
| Browserstack                                                            | Allows testing on different devices                                                                                                               |
| [GitHub](https://help.github.com)                                       | Version control / code management                                                                                                                 |
| GChat                                                                   | Realtime messaging for those not on Slack                                                                                                         |
| [Google Analytics](https://analytics.google.com) and Google Tag Manager | Web traffic analytics                                                                                                                             |
| Google Forms                                                            | Form generator/manager                                                                                                                            |
| Google Meet                                                             | City's main form of video conferencing                                                                                                            |
| Google Optimize                                                         | A/B testing                                                                                                                                       |
| Invoice Cloud                                                           | Main payment processor for the City; managed by Enterprise but we link out/often confusion on if a website problem or problem with this platform. |
| Lando                                                                   | Containerized build tool/utility                                                                                                                  |
| [Percy.io](https://docs.percy.io/docs)                                  | Visual regression testing                                                                                                                         |
| PHPStorm                                                                | JetBrains IDE for PHP (for Drupal)                                                                                                                |
| [Postmark](https://postmarkapp.com/developer)                           | Sending transactional emails                                                                                                                      |
| [Rollbar](https://docs.rollbar.com/docs/javascript)                     | Server logging                                                                                                                                    |
| [Siteimprove](http://siteimprove.com)                                   | Quality Assurance and behavior mapping                                                                                                            |
| [Slack](https://get.slack.help)                                         | Realtime messaging                                                                                                                                |
| [Swiftype](https://www.swiftype.com)                                    | Powers global search for boston.gov                                                                                                               |
| Stripe                                                                  | Payment processor used for Registry suite                                                                                                         |
| Terraform                                                               | AWS Infrastructure Scripting                                                                                                                      |
| [TravisCI](https://docs.travis-ci.com)                                  | Continuous integration                                                                                                                            |
| Upaknee                                                                 |                                                                                                                                                   |
| [updown.io](https://updown.io/api)                                      | Uptime monitoring                                                                                                                                 |
| Visual Studio Code                                                      | Developer IDE for javascript etc                                                                                                                  |
| Webex                                                                   | City's main platform for large video presentations and conferencing (if Google Meet doesn't accommodate)                                          |

Information on which team members have access to these can be found here: [https://docs.google.com/spreadsheets/d/1pn9mYnRKtfudlX9VihBWUSuvhJDsiu571g-Dh-j5kss/edit#gid=0](https://docs.google.com/spreadsheets/d/1pn9mYnRKtfudlX9VihBWUSuvhJDsiu571g-Dh-j5kss/edit#gid=0)
