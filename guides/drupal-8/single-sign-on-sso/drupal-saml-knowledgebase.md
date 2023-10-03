---
description: This has been updated for Drupal 10, as at October 2023.
---

# Drupal SAML Knowledgebase

{% hint style="info" %}
#### Important Terminology.

**IDP**: _Identity Provider_ - The IDP is Ping Federate which is managed by the City's IAM team. The actual user accounts are stored in Microsoft Active Directory and other City managed domain services which are connected to Ping. The IDP's function is to provide the user "authentication" checking user credentials and reporting  security roles - reducing or removing the SP 's responsibility for verifying access and maintaining the security element of user accounts.

**SP:** _Service Provider_ - The SP can be thought of as as a series of authentication functions within an application. In most implementations, the IDP and SP communicate using a SAML protocol. On successful authentication with the IDP, the SP receive standard and authoritative message (usually via a SAML formatted cookie) defailing the users name, email and (ideally but optionally) the Users security roles/groups.
{% endhint %}

{% hint style="info" %}
#### SAML Implementation in Drupal.

In the case of boston.gov the SP is Drupal, or more specifically the SamlAuth module in Drupal.&#x20;

The [SamlAuth](https://www.drupal.org/project/samlauth) module ([config-UI here](https://content.boston.gov/admin/config/people/saml)) manages the communication with Ping (IDP) and exchanges SAML messages during a standard SAML process.

The IDP is used only to authenticate the user and SamlAuth receives a SAML message back from Ping (IDP) containing the username, firstname, lastname and email only of the authenticated user.  If Authentication at Ping fails, the user cannot login to Drupal.

* SamlAuth maps user accounts from Ping with user accounts in Drupal, where the `cn` in Ping equals the `username` in Drupal.  If no matching account exists, SamlAuth creates an account in Drupal, and assigns the `content_author`role.&#x20;
* Security Groups in Ping are not reported to Drupal/SamlAuth, so no attempt is made to synchronize roles between Ping and Drupal.  Even though it is not done at this time, this can be simply implemented in the future. Therefore changes to user groups in Ping and Drupal are independent and never synchronized.
* Drupal requires an email address to create an account. The email address does not need to be on the bostomn.gov domain, and it does not need to be unique.  Ping (actually the IAM authoritative record systems behind Ping) cannot guarantee to provide an email address, so SamlAuth has been modified to create a "fake" email address using the pattern `username@boston.gov`.

SamlAuth requires an x509 security certificate to be issued from the IDP (Ping) in order to verify authenticity of the SAML response, and to decrypt the SAML message contents.  This x509 certificate is issued by IAM and is managed by the [Key](https://www.drupal.org/project/key) module.
{% endhint %}

{% hint style="info" %}
There are 2 IDP metadata certificates one for prod and one for non-prod. &#x20;

* The Prod metadata connects to sso.boston.gov.&#x20;
* The Non-prod environments all connect to the IAM Test environment at sso-test.boston.gov.

Boston.gov users typically use Access Boston to login to boston.gov (Drupal).&#x20;

* The tile on Access Boston Production launches content.boston.gov&#x20;
* The tile on Access Boston Test launches d8-stg.boston.gov.
* To use SSO to login to any other environment, you need to use the saml link. For example:
  * Boston.gov dev => https://d8-dev.boston.gov/saml/login
  * Boston.gov UAT => https://d8-uat.boston.gov/saml/login
  * Boston.gov local => https://boston.lndo.site/saml/login
{% endhint %}

{% hint style="success" %}
### Update IDP Certificate

From time to time the IDP certificate will expire and need to be re-issued by the Identity and Access Management team.

You should request the `IDP Metadata XML` from IAM. This info will come as a single file per environment - with the extension .xml. See the next box to work out which part of the metadata is the certificate.

**For Acquia Environments**

1. Login to the Acquia Cloud Console
2. Navigate to the environment you wish to update, and select the `variables`section
3. Update the environment variable with the new certificate (cut and paste it over the existing entry) and save.
4. **Note:** The change will be immediate on the environment.

**For your Local Dev Environment**

Your local dev environment uses the IDP Metadata from the IAM Test environment.

1. Login to your local Drupal
2. On [this page](https://boston.lndo.site/admin/config/system/keys), edit the key you wish to update
3. On the key page, in the certificate box, paste the new x509 certificate
4. Save the page.
{% endhint %}

{% hint style="info" %}
### Finding the certificate in the metadata

The certificate is the `X509Certificate` field in the IDP Metadata file.

The IDP Metadata file (XML) can be opened in any text editor, and it will look as follows:

<pre><code>  $metadata['urn:XXX:sso:boston.gov'] = array (
  
  ...
  
  'keys' => array (
      0 => array (
        'encryption' => false,
        'signing' => true,
        'type' => 'X509Certificate',
        'X509Certificate' => <a data-footnote-ref href="#user-content-fn-1">'####new-cert-string'</a>,
      ),
    ),
    
    ...

  );
</code></pre>
{% endhint %}

{% hint style="success" %}
**Tip**

You can check the expiry of the IDP certificate by clicking the `info` button the [key entry](https://content.boston.gov/admin/config/system/keys) console.
{% endhint %}

[^1]: This is the certificate  &#x20;
