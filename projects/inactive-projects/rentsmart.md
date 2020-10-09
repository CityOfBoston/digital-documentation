---
description: As of 2019
---

# Rentsmart

**Project Background**

* Project came out of Department of Neighborhood Development \(DND\)
* Qlarion built
* When originally developed open data portal didn’t exist/wasn’t in its current state to be used could use ckan API
* Launched publicly early August 2017 - no known issues or outages since launch

**Iterations**

* Been rebuilt a few different times
* Original version was built with additional data sources, but was parred down to show valuable info quickly
* Initially very GIS focused

**Technology used**

* Hosted on heroku \[migrated from Qlarion to City's heroku in July 2018\]
* Uses heroku resources - uses single production dyno
* Could be moved to AWS \[down the road\], but would need to create a PHP container in the current AWS setup
* Built in php and javascript
* Bootstrap front end \[Sebastian advised on this at some point\]
* Data services \(SAP\) sits on City side. Every night loads/nightly job into postgres database \(data services drops data then PHP to post gres to stage the data/make it look nice\). SAP = ATl platform. PHP script to get data to website.
* Assurance that the data runs before page load is set within the code.
* index.html - served on apache

**Some known data sources:**

* ISD
* Lagan/311
* Assessing
* Open question: Are there more?

**Areas for improvement**

* Adjust dynos and other heroku resource \(to bring cost of app down\)
* Adjust data source/feed set up  
* Revisit entire structure: Qlarion estimated would probably take 1 week of time to switch the data sources and restructure data

