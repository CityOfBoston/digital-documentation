# Registry Suite

The City of Boston's Registry Department manages birth, marriage, and death records for Boston with records dating as far back as 1630. They complete more than 100,000 transactions a year. Their pages are some of the most trafficked parts of [Boston.gov](https://www.boston.gov/) and provide vital services to constituents. Patty McMahon is the main end stakeholder for this.

The Department of Innovation and Technology’s \(DoIT\) Digital Team brought these records online. 

[Birth Certificates](https://app.gitbook.com/@boston/s/digital/~/drafts/-M11u2Ds-oY83bP9j3-S/projects/inactive-projects/registry-suite/birth-certificates)

[Marriage Certificates](https://app.gitbook.com/@boston/s/digital/~/drafts/-M12QwtqGTBk6JHr0qtY/projects/inactive-projects/registry-suite/marriage-certificates)

[Death Certificates](https://app.gitbook.com/@boston/s/digital/~/drafts/-M12SNjLUStJD150EY-A/projects/inactive-projects/registry-suite/death-certificates)



When these applications are worked on Rich Oliver and Georges Hawat should be in the loop so that we ensure database or back end application changes happen alongside Digital development. Scott Blackwell can be a resource for this as well.



Along the way, items got added to certain of the three apps, but at times they didn't get completely added. 

Items that need to be address/re-addressed: [https://github.com/orgs/CityOfBoston/projects/2](https://github.com/orgs/CityOfBoston/projects/2)

Small insight into backend applications \(video\): [https://drive.google.com/file/d/1vw\_jiPGUUd8vwx9yemEqAjKkeShqkQlK/view?usp=sharing](https://drive.google.com/file/d/1vw_jiPGUUd8vwx9yemEqAjKkeShqkQlK/view?usp=sharing)



**Payment information**

The applications are connected to Stripe for payment. We've explored using InvoiceCloud, but the functionality needed isn't developed/offered by InvoiceCloud. They say they're developing it and it will be usable by the end of 2021. There are ongoing conversations and contracting needs about this.  The Administration and Finance Cabinet \(Treasury\) should always be looped in on these conversations.

International ordering is an interesting challenge for the City; these apps don't currently support international ordering/shipping. Insight from the Registry office: When a user tries to put in an international address it won't accept it. To workaround this, they advise constituents to enter the following information as the shipping address: Boston, MA 99999. This alerts staff that it is an international order. When the user gets the confirmation email, they ask that the constituent please reply to it with the full address that you want the record mailed to. After that is received, Registry processes the order. They haven't flagged this as a super high priority item, so we have not chosen to prioritize/implement.

