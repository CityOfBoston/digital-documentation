---
description: Periodically SSL certificates need updating.
---

# Updating SSL Certificates

## Digitcert Issued Certificates

To perform these tasks, you will need to obtain from the **Security Team**, the following:

* the `SSL Certificate` \(possibly a `.crt` file\), 
* the `SSL private key` \(a `.key` file\), and
* one or more`CA intermediate certificates` \(possibly `.pem` files\).  _\(If there are multiple intermediate certs, you can cut and paste them into a single file with each cert starting on a new line below the old cert.  The order is not important.\)_

All of these files should be readable in a text editor.

### Acquia - boston.gov

The new/updated certificate is updated directly on the Acquia Cloud web console.

**To install the new certificate:**

1. Login to the [cloud console](https://cloud.acquia.com/login), click **Develop** in top menu.
2. Select an **Organization** "_City of Boston_"\), **Application** \("_bostond8_"\) and **environment** \(e.g. "_Dev_"\).
3. In the left column menu, select **SSL**
4. Click the "**Install SSL Certificate**" button
5. Provide a sensible label \(for example _\*.boston.gov 20xx_\)
6. Open the files provided and insert their contents into the text boxes.  **Note:** The files will all have text in 80 char wide columns and start with text`---- BEGIN CERTIFICATE/RSA KEY ----` 
7. Click the "**Install"** button
8. You should see a summary page with the old certificate \(active\) and the new certificate you just installed \(inactive\). When you are ready, you can _Activate_ the new certificate, and then _Deactivate_ and _Remove_ the old certificate.

### AWS - webapps \(incl. registry.boston.gov\)

{% hint style="info" %}
AWS manages SSL certificates in its **Certificate Manager** service.  
{% endhint %}

The new/updated Digicert certificate can be loaded into the AWS Certificate Manager and then applied to various services within AWS as required.

1. Login to the [AWSConsole](https://console.aws.amazon.com/).
2. Navigate to the **Certificate Manager**.
3. Locate the _\*.boston.gov_ certificate in the list.  **Importantly:** The correct entry will have a Type=Imported.
4. Expand the entry and click the "**Reimport Certificate"** button.
5. Cut and past the certificate parts onto the page and click "**Review and Import**".

AWS Services which used the old certificate will now use the new one without any further changes required.

{% hint style="danger" %}
_04Oct2021_.  The registry app uses an AWS issued certificate.  
 **=&gt; This will be changed during 10/2021.**
{% endhint %}

### IIS - cityofboston.gov

The cityofboston.gov certificate is installed on the Imperva load balancer/WAF.  Digital do not need to install the certificate on the IIS server at zpcobweb01.

## AWS Issued Certificates

{% hint style="info" %}
AWS manages and issues SSL certificates in its **Certificate Manager** service.  
{% endhint %}

AWS hosted webapps use certificates managed by AWS.  All webapps without permanent **\*.boston.gov** URLs use AWS issued certificates.

* \*.digital-public.boston.gov and 
* \*.digital-staging.boston.gov.

AWS automatically renews expiring certificates and sends out an email to _webmaster@boston.gov._  Simply click on the approval link in the email to allow the cert to be automatically regenerated and installed.

{% hint style="success" %}
Someone in the digital needs to be in the webmaster@boston.gov email group, and needs to have the AWS policy [AWSCertificateManagerPrivateCAPrivilegedUser](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSCertificateManagerPrivateCAPrivilegedUser) or [AWSCertificateManagerFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSCertificateManagerFullAccess) in order to receive the emails and authorize the certificate renewal.
{% endhint %}

