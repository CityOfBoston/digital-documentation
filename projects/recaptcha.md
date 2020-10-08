---
description: >-
  The Google reCAPTCHA v3 returns a score for each request without user
  friction. The score is based on interactions with your site and enables you to
  take an appropriate action for your site.
---

# Google reCAPTCHA

**Install Google reCAPTCHA**

* Make sure to register reCAPTCHA v3 keys \(Secret and Site\)
* Follow the installation and configuration instructions here to add reCAPTCHA to your Drupal site: [https://www.drupal.org/docs/8/modules/recaptcha-v3/installation-and-configuration](https://www.drupal.org/docs/8/modules/recaptcha-v3/installation-and-configuration)  
* 
**reCaptcha analytics**

* Url to view scores: [https://www.google.com/recaptcha/admin/site/430165885](https://www.google.com/recaptcha/admin/site/430165885)

![](../.gitbook/assets/image%20%2824%29.png)

**Environment variables** 

* All variables are in Acquire: [https://cloud.acquia.com/a/develop/applications/](https://cloud.acquia.com/a/develop/applications/)
* To update environment variables properly, make sure to update private repo with the current variable name and then add to Acquire. Only add Api key to Acquire. 
* Make sure you add the "recaptcha\_v3.settings" in gitignore.



