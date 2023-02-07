---
description: >-
  A generic form which is attached to email addresses found on boston.gov, and
  handles sending emails to those addresses.
---

# Contact Form

{% hint style="success" %}
This app uses the `bos_email` provided services as [described here](../drupal-8-micro-services-api-end-points/postmark-email-services.md).
{% endhint %}

#### Overview

1. A contact us form template is maintained (within script tags) in `bos_theme/templates/snippets/contactFormTemplate.html.twig` and is included on **every** boston.gov (Drupal) page.
2. The patterns library contact form javascript function `start()`(in `scripts/components/contact.js` ) is executed when a boston.gov (Drupal) page loads.  \
   The `start()` function scans the completed page looking for email addresses anywhere in the html being served.  Essentially, it:\
   \- replaces the default `mailto` directive with a click event listener which will trigger the `handleEmailClick()` function and \
   \- attaches a click event listener to the submit button so the `handleFormSubmit()`function is called when the user clicks the submit button on the contact form.

{% hint style="warning" %}
**TODO**: The javascript is called once the page has finished loading.  It should be extended to also run when ajax events return data to the page, since email addresses could be served by XHR/AJAX as well as traditional document events.
{% endhint %}

1. When an email address is clicked on the page, `handleEmailClick()`\
   ``- copies the template form from the script tags, \
   \- inserts the correct email recipient to a hidden field, \
   \- inserts all this onto the page and displays the contact us form, and\
   \- in the background makes an ajax request to `/rest/email_token/create,` generating and saving a unique "session" token in the form.
2. When the submit button is clicked, `handleFormSubmit()` validates the form, and then submits, along with an authorization token to `/rest/email_session/contactform`.

{% hint style="info" %}
The `PostmarkAPI.php` in the `bos_email` module provides a "guaranteed delivery" type service.  It tries to send the email via the postMark API, and if it fails for some reason, queues the email for later delivery in a Drupal queue.\
Drupal will retry the email until it is able to send it to the postMark service.
{% endhint %}

{% hint style="info" %}
Email tracking with Drupal has been discontinued with the use of the PostMark service.  All logging can be obtained from the PostMark UI/Console.

Emails which fail to send can be viewed in the email\_contactform queue.
{% endhint %}

\=====

Emails are sent from an email address that is generated for each email sent. The format of the email address is:

{random\_string}@contactform.boston.gov

**We don't send the email from the original sender's email address as that could be a vector for an email spoofing attack.**

The reply\_to header of the email is set to the constituents email first and the unique address of the contact form second. When someone replies to the email the to address of the email is set to the constituents email first and the unique address second. This delivers two copies of the email. One goes directly to the constituent, and one to the contact form API. We log the response time using the copy that gets sent to the contact form API. Once the reply email is delivered to the constituent, further replies will be direct between the constituent and city.

[Diagram of the overall flow](https://user-images.githubusercontent.com/34665739/34566740-b4a90256-f12d-11e7-8162-f2abaaba7454.png) (needs updating)
