---
description: >-
  Boston.gov authenticates users against a City of Boston idP provided by Ping
  Federate service, managed by Security & IAM team.
---

# Single Sign On (SSO)

The broad workflow is as follows:

![](../../../../.gitbook/assets/sso-workflow-boston.gov.png)

### Workflow from AccessBoston

* AccessBoston has 2 icons, one for boston.gov (content editor) and one for hub.
* Users are authenticated by Ping and then are redirected to:
  * in the case of **boston.gov** the users workbench
  * in the case of **HUB** the hub homepage on boston.gov (probably aka hub.boston.gov)

### Workflow from Drupal (i.e. direct boston.gov url access)

#### _User is already authenticated from Ping_ (browser already has an unexpired session from Ping)

* User is directed to the page they have requested.

#### _User has no valid session from Ping._

* If the page requested is not secure, then the user goes directly to the page.
* If the page requested is secure, then the user is redirected to Ping Federate and then redirected back to the page requested.
