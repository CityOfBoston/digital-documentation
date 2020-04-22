---
description: Emergency Messaging Platform
---

# Everbridge API + UI

The City of Boston switched from CodeRed to Everbridge for emergency notifications \(mainly text messages\) in April 2020.

For historical information on emergency alert sign ups see here: [https://app.gitbook.com/@boston/s/digital/projects/decommissioned-apps-or-services/codered](https://app.gitbook.com/@boston/s/digital/projects/decommissioned-apps-or-services/codered)

**Notes from initial call re: API on 4/21/20 found here:**

* * Open RESTAPI &gt; json object
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

**Scope for Version 1 \(installing API\):**  
1. Swap Everbridge API in for CodeRed API on boston.gov \(there is an emergency notifications component. Determine if email or phone is unique identifier \(still TBD\).  
2. Add additional address fields so that address is address line \(street number and name\), city, state, and zip code \(rather than just zip code\)  
3. Add additional languages to drop down 

**Future Enhancements:** 

1. Ideally, a General Assembly UXDI group reviews this component/sign up process. To be determined.
2. Potentially, how is the component translated so people know that other languages are available and can be selected in the dropdown. 

