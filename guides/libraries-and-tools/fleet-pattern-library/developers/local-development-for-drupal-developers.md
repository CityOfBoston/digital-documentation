---
description: Instructions on developing css and js for Drupal developers.
---

# Local Development for Drupal Developers

you will be able to view fleet on [`https://patterns.lndo.site`](https://patterns.lndo.site/)[13:26](https://cityofboston-doit.slack.com/archives/DQBS5V45U/p1588872394012600)and your local [boston.gov](http://boston.gov/) on [`https://boston.lndo.site`](https://boston.lndo.site/)- but it will now use css and js from patterns.lndo.site (the patterns container acting as a CDN) (edited) [13:27](https://cityofboston-doit.slack.com/archives/DQBS5V45U/p1588872429013200)you will find the patterns files in `reporoot/patterns`[13:28](https://cityofboston-doit.slack.com/archives/DQBS5V45U/p1588872495014500)Gulp and stencil processes in the patterns container will monitor that folder for changes, and immediately rebuild the fleet website and put the js and css (ie. the CDN) into `reporoot/patterns/public`. (edited) [13:31](https://cityofboston-doit.slack.com/archives/DQBS5V45U/p1588872682017700)the boston  and patterns repos are managed separately by git and commiting changes to the files you make should automatically apply to the correct repo.[13:31](https://cityofboston-doit.slack.com/archives/DQBS5V45U/p1588872705018300)(i.e patterns changes to patterns repo, and d8 changes to the boston repo)
