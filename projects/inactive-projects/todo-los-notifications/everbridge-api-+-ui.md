---
description: Emergency Messaging Platform
---

# Everbridge API + UI

_Swapping the API for boston.gov/alerts from codered to everbridge is ready to go, but waiting on approval from OEM - as of 5/4/2020_

The City of Boston switched from CodeRed to Everbridge for emergency notifications \(mainly text messages\) in April 2020.

For historical information on emergency alert sign ups see here: [https://app.gitbook.com/@boston/s/digital/projects/decommissioned-apps-or-services/codered](https://app.gitbook.com/@boston/s/digital/projects/decommissioned-apps-or-services/codered)

**Boston.gov pages where API appears:**

1. Production: boston.gov/alerts &gt; [https://www.boston.gov/departments/emergency-management/city-boston-alerts-and-notifications](https://www.boston.gov/departments/emergency-management/city-boston-alerts-and-notifications) \(connected the production Everbridge API\)
2. Development: d8-dev.boston.gov/alerts &gt; [https://d8-dev.boston.gov/departments/711/city-boston-alerts-and-notifications](https://d8-dev.boston.gov/departments/711/city-boston-alerts-and-notifications) \(connected to the test API\)

**City of Boston organization ID’s are:**  
ALERTBoston: 892807736721548   
Alert Boston API Testing: 454102597238915

**Contact at Everbridge:**  
Chris Rigby   
Implementation Team Lead   
Office: 781-819-5161   
Cell: 781-771-4590   
chris.rigby@everbridge.com

**Notes from initial call re: API on 4/21/20 found here:**

* Open RESTAPI &gt; json object
* First, last name, unique ID \(email or phone\), record type
* What are all the available fields &gt; /contacts in [Api.everbridge.net](http://Api.everbridge.net)
* API user provisioned at user level &gt; don’t log in to the ever bridge UI b/c this will break the API connection
* Real time syncs or batch uploads
* Up 40 API calls per minute
* To test API: can create another ‘organization’ &gt; can be done in about 5 minutes. Chris will go ahead and create this now.
* We have one ‘organization’ right now
* Chris, ever bridge implementation partner
* REST API programming guide available
* Need to set up/build on [boston.gov](http://boston.gov) component &gt; should be the 10 languages set up via text with everbridge
* Can geolocate, but would need full address

**Scope for Version 1:**  
1. Swap Everbridge API in for CodeRed API on boston.gov \(there is an emergency notifications component. Determine if email or phone is unique identifier \(still TBD\).  
2. Add additional address fields so that address is address line \(street number and name\), city, state, and zip code \(rather than just zip code\)  
3. Add additional languages to drop down, so that the dropdown includes 10 languages, which are and should be in this order: English, Español, Kreyòl ayisyen, 中文, Tiếng Việt, Kriolu, Русский, عربية\#, Português, Français, Af Soomaali

**Future Enhancements:** 

1. Ideally, a General Assembly UXDI group reviews this component/sign up process. To be determined.
2. Potentially, how is the component translated so people know that other languages are available and can be selected in the dropdown. 

