---
description: Periodically SSL certificates need updating.
---

# Updating SSL Certificates

## Digitcert Issued Certificates

### Acquia - boston.gov

The certificate is updated directly on the Acquia Cloud web console.

To perform this task, you will need to obtain from the **Security Team**, the following:

* the `SSL Certificate` \(possibly a `.crt` file\), 
* the `SSL private key` \(a `.key` file\),
* one or more`CA intermediate certificates` \(possibly `.pem` files\).  _\(If there are multiple intermediate certs, you can cut and paste them into a single file with each cert starting on a new line below the old cert.  The order is not important.\)_

All of these files should be readable in a text editor.

**To install the new certificate:**

1. Login to the cloud console, click **Develop** in top menu.
2. Select an **Organization** "_City of Boston_"\), **Application** \("_bostond8_"\) and **environment** \(e.g. "_Dev_"\).
3. In the left column menu, select **SSL**
4. Click the "**Install SSL Certificate**" button
5. Provide a sensible label \(for example _\*.boston.gov 20xx_\)
6. Open the files provided and insert their contents into the text boxes.  **Note:** The files will all have text in 80 char wide columns and start with text`---- BEGIN CERTIFICATE/RSA KEY ----` 
7. Click the "**Install"** button
8. You should see a summary page with the old certificate \(active\) and the new certificate you just installed \(inactive\). When you are ready, you can _Activate_ the new certificate, and then _Deactivate_ and _Remove_ the old certificate.

### AWS - webapps \(incl. registry.boston.gov\)

04Oct2021.  The registry app uses an Amazon managed certificates. **This will be changed during 10/2021.**

04Oct2021. Other webapps use Amazon Managed Certificates.

AWS manages certificates in its **Certificate Manager** service.  AWS automatically renews expiring certificates and sends out an email.  Simply click on the approval link in the email to allow the cert to be regenerated and installed.

### IIS - cityofboston.gov

The cityofboston.gov certificate is installed on the Imperva load balancer/WAF.  Digital do not need to install the certificate on the IIS server at zpcobweb01.

## AWS Issued Certificates

