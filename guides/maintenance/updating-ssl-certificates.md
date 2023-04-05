---
description: Periodically SSL certificates need updating.
---

# Updating SSL Certificates

## Checklist: Domains managed by Digital

* [ ] www.boston.gov - city's public website hosted by Acquia,
* [ ] registry.boston.gov - registry suite of apps hosted on AWS's ECS service,
* [ ] hub.boston.gov - city's intranet hosted  on AWS's ElasticBeanstalk service,
* [ ] cityofboston.gov - city's legacy website, legacy apps hosted on IIS as ZPCOBWEB01,
* [ ] park.boston.gov - certificate for the Park Boston app hosted by Passport (external Vendor),
* [ ] microservices/apps - utilities hosted on AWS's ECS service.

{% hint style="success" %}
Ideally certificate cycling activities should be conducted during the weekly Digital maintenance window.\
If that is not possible, then a Change Management ticket should be raised, (even if retrospectively) b/c these are production changes.&#x20;
{% endhint %}

## Digitcert Issued Certificates

To perform these tasks, you will need to obtain from the **Security Team**, the following:

* the `SSL Certificate` (possibly a `.crt` file),&#x20;
* the `SSL private key` (a `.key` file), and
* one or more`CA intermediate certificates` (possibly `.pem` files). \
  _(If there are multiple intermediate certs, you can cut and paste them into a single file with each cert starting on a new line below the old cert.  The order is not important.)_

All of these files should be readable in a text editor.

### Acquia - boston.gov

The new/updated certificate is updated directly on the Acquia Cloud web console.

This process should be repeated on all environments on Acquia.  \
It is _recommended_ that the certificate first be applied to the `dev` environment so that any issues can be discovered on a non-prod resource ...

**To install the new certificate:**

1. Login to the [cloud console](https://cloud.acquia.com/login), click **Develop** in top menu.
2. Select an **Organization** "_City of Boston_"), **Application** ("_bostond8_") and **environment** (e.g. "_Dev_").
3. In the left column menu, select **SSL**
4. Click the "**Install SSL Certificate**" button
5. Provide a sensible label (for example _\*.boston.gov 20xx_)
6. Open the files provided and insert their contents into the text boxes.  **Note:** The files will all have text in 80 char wide columns and start with text`---- BEGIN CERTIFICATE/RSA KEY ----`&#x20;
7. Click the "**Install"** button
8. You should see a summary page with the old certificate (active) and the new certificate you just installed (inactive). When you are ready, you can _Activate_ the new certificate, and then _Deactivate_ and _Remove_ the old certificate.
9. Repeat for each Acquia environment, (stage, ci, uat,dev2 & **prod**)

### AWS - WebApps & TheHub

{% hint style="info" %}
AWS manages SSL certificates in its **Certificate Manager** service. &#x20;
{% endhint %}

#### WebApps (incl. registry.boston.gov)

The new/updated Digicert certificate can be loaded into the AWS Certificate Manager and then applied to various services within AWS as required.

1. Login to the [AWSConsole](https://console.aws.amazon.com/).
2. Ensure you are in the `us-east-1`region
3. Navigate to the **Certificate Manager**.
4. Locate the _\*.boston.gov_ certificate in the list.  **Importantly:** The correct entry will have a Type=Imported.
5. Expand the entry and click the "**Reimport Certificate"** button.
6. Cut and past the certificate parts onto the page and click "**Review and Import**".

AWS Services which used the old certificate will now use the new one without any further changes required.

{% hint style="danger" %}
_04Oct2021_.  The registry app uses an AWS issued certificate.\
&#x20;**=> This will be changed during 10/2021.**
{% endhint %}

#### The Hub (hub.boston.gov)

The Hub is hosted on AWS using the ElasticBeanstalk service: It is in region `us-east-2`.

1. Login to the [AWSConsole](https://console.aws.amazon.com/).
2. Ensure you are in the `us-east-2`region
3. Navigate to the **Certificate Manager**.
4. Locate the _\*.boston.gov_ certificate in the list.  **Importantly:** The correct entry will have a Type=Imported.
5. Expand the entry and click the "**Reimport Certificate"** button.
6. Cut and past the certificate parts onto the page and click "**Review and Import**".

The hub will now use the new certificate without any further changes required.

### IIS - cityofboston.gov

The cityofboston.gov certificate is installed on the Imperva load balancer/WAF.  Digital do not need to install the certificate on the IIS server at zpcobweb01.

#### **Video streaming**

The wowza service on ZSTREAMING requires the updated cityofboston.gov certificate to be added/updated in the keystore.&#x20;

See [here](https://www.wowza.com/docs/How-to-request-an-SSL-certificate-from-a-certificate-authority) or [here](https://www.wowza.com/docs/how-to-import-an-existing-ssl-certificate-and-private-key)

## AWS Issued Certificates

{% hint style="info" %}
AWS manages and issues SSL certificates in its **Certificate Manager** service. &#x20;
{% endhint %}

AWS hosted webapps use certificates managed by AWS.  All webapps without permanent **\*.boston.gov** URLs use AWS issued certificates.

* \*.digital-public.boston.gov and&#x20;
* \*.digital-staging.boston.gov.

AWS automatically renews expiring certificates and sends out an email to _webmaster@boston.gov._  Simply click on the approval link in the email to allow the cert to be automatically regenerated and installed.

{% hint style="success" %}
Someone in the Digital Team needs to be in the webmaster@boston.gov email group, and needs to have the AWS policy [AWSCertificateManagerPrivateCAPrivilegedUser](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSCertificateManagerPrivateCAPrivilegedUser) or [AWSCertificateManagerFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess) in order to receive the emails and authorize the certificate renewal.
{% endhint %}

## External Vendors

### park.boston.gov (Vendor - Passport)

{% embed url="https://www.passportinc.com" %}

Email: james\_loper@passportinc.com - support@passportinc.com\
Alt: [mu.saleem@passportinc.com](mailto:mu.saleem@passportinc.com).\
Technical: jeremy.proffitt@passportinc.com

Need to send Passport a copy of the certificate so that they can add it to the website (and possibly app) they host for parking in boston.

