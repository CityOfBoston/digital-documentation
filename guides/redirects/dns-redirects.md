---
description: How we handle redirecting unused domains to their new homes
---

# DNS Redirects

We often get called upon to take old (sub)domains, such as data.cityofboston.gov or "vanity" domains like imagine.boston.gov and publicartsboston.com, and point them to new homes. Typically, but not always, these redirects are to pages on Boston.gov.

Making such a redirect is a multi-step process:

1. Verify that the Acquia production environments' [Domain Management](https://cloud.acquia.com/a/environments/59916-5ad427f5-60d6-48fd-983e-670ddc7767c4/domain-management/domains) section contains an entry for the domain to be redirected, then
2. Configure Drupal to handle the redirection using the [Domain Redirect](https://content.boston.gov/admin/config/search/redirect/domain) module, then
3. Change the CNAME DNS record at DNS Made Easy to point to www.boston.gov.

### Acquia Domain Management

[Login to Acquia](https://accounts.acquia.com/sign-in?site=cloud\&path=a), and then navigate to the Production environment.

In the left hand menu, navigate to "Domain Management"

On the Domain Management page, you will see a list of Domains that the environment will accept internet traffic for.  These domains will all effectively be considered aliases for `www.boston.gov` and will be redirected to the Drupal docroot for `boston.gov`.  However, after the redirect the header will retain information on the orginal domain requested, and Drupal can use this to further redirect traffic on to the desired destination (as defined in the next section).

If the domain you are trying to redirect is not going to be matched by actual (or wildcard) entries already in the list of Domains on the page, then click the "Add Domain" button and add your domain and save.  _Nothing will happen until you complete the DNS Changes step below._

### Drupal Domain Redirect

Login to `content.boston.gov` via [access boston](https://access.boston.gov), and then navigate to `Configuration | Search and Metadata | URL redirects`.  From that Redirect page, click on the `Domain redirects` tab in the page submenu.

In the bottom half of this page, you will now see a list of subdomains / paths, and final destinations that are already configured.  The "From domain" is a domain which will resolve from the list on the Acquia Domian Management page above (either exactly or via a wildcard).  The "Sub path" can be any path on that domain/subdomain or "/\*" to redirect the whole site. Finally, the "Destination" is the redirect target.  If the user enters a url matching the from domain and sub path they will be redirected to the Destination.  &#x20;

&#x20;You can see examples in the instructions in the top half of the page. Pay attention to the flexibility offered by using wildcards in the "sub path" field.

_Redirects on this page take effect as soon as they are added or updated, provided the DNS is in place._

{% hint style="warning" %}
**Care:** Take care that the "rules" on the page are applied from to to bottom. If your redirect is not working properly, it could be because a rule with a wildcard condition is being met exists higher in the list than the rule you have added.
{% endhint %}

{% hint style="info" %}
**Tip:** The destination does not have to be a page hosted by the `boston.gov` Drupal site, it can be a redirect to any valid URL.
{% endhint %}

{% hint style="danger" %}
You can expect new redirects to take effect immediately, as nothing is cached.  \
If the redirect is not updating after you have changed it, then it could be:  \
\- because the users local cache is not refreshing (so try clearing that first), or \
\- because the varnish cache is holding the page b/c the purgers may not acknowledge redirect changes as content changes and the pages may not be invalidated.  (try clearing the varnish cache)\
\- some other cache (e.g. Imperva) is holding the redirect. (this is much less common)
{% endhint %}

### DNS Changes

Most domains or subdomains to be redirected which considered under this documentation are hosted on DNSMadeEasy and therefore use DNSMadeEasy nameservers.  It is possible that domains are hosted on other services, such as GoDaddy.  If you are unsure where the domain is hosted, start by contacting infrastructure who may be able to help with where the DNS records need to be altered.

All DNS changes have to be done through a Change Management request, though given their low risk they typically can be turned around same-day.  Requests for changes to DNS also require a service desk/trackit ticket. &#x20;

&#x20;Contact Infrastructure to find out who the correct contact is for making DNS changes - at the time of writing Frank Skelton is usually responsible for making the changes in DNSMadeEasy.&#x20;

#### Service Desk Trackit Ticket

Send an [email to the DoIT service desk](mailto:doitservicedesk@boston.gov) (doitservicedesk@boston.gov), and cc the Infrastructure contact (Frank) in. The email should outline the subdomain you need to add/change and specify the record should be a `CNAME` target to `www.boston.gov`.

#### Change Management Ticket

Create a new [Change Management ticket](https://app.smartsheet.com/b/publish?EQBCT=591c5a77db1f47baa7bf2a80d6039fe3) in the smartsheets system.  Set yourself and the Infrastructure contact (Frank) as the Contact Persons. your manager as the Senior Approving Manager and Frank as a reviewer.  For DNS changes it is acceptable to set the date to be today if you want the change to be immediate.



