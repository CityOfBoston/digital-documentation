# SAML Checklist

* [ ] The link in access-boston needs to be updated to `https://hub.boston.gov/saml_login`
* [ ] Anonymous users accessing any page are redirected to `/saml_login`
* [ ] Ping-authenticated user arriving at `saml_login` has new account created for them if there isn't already one.

This will cause the following use-cases:

1. Any ping-unathenticated user hitting any page on the hub will be directed to sso page.
2. Any ping-authenticated user \(i.e. most likely has already logged in via access boston\) hitting the hub will have access, an account being automatically made for them if it does not already exist.
3. Clicking Hub icon on access boston takes user straight to the drupal/hub front page
4. Returning from sso.boston.gov user goes to front page.



