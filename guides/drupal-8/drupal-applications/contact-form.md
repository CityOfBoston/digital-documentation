---
description: >-
  A generic form which is attached to email addresses found on boston.gov, and
  handles sending emails to those addresses.
---

# Contact Form

{% hint style="success" %}
This app uses the `bos_email` provided services as [described here](../drupal-8-micro-services-api-end-points/postmark-email-services.md).
{% endhint %}

## Overview

1. A contact us form template is maintained (within script tags) in `bos_theme/templates/snippets/contactFormTemplate.html.twig` and is included on **every** boston.gov (Drupal) page.
2. The patterns library contact form javascript function `start()`(in `scripts/components/contact.js` ) is executed when a boston.gov (Drupal) page loads.  \
   The `start()` function scans the completed page looking for email addresses anywhere in the html being served.  Essentially, it:\
   \- replaces the default `mailto` directive for each email address with a click event listener which will trigger the `handleEmailClick()` function, and \
   \- attaches a click event listener to the forms' submit button and calls the `handleFormSubmit()`function when the user clicks the submit button on the contact form.

{% hint style="warning" %}
**TODO**: The `handleEmailClick()` function is called once when the page has finished loading.  \
It should be extended to also run when ajax events return data to the page (since email addresses could be served by XHR/AJAX as well as traditional document events)..

@see [DIG-910](https://bostondoit.atlassian.net/browse/DIG-910)
{% endhint %}

3. When an email address is clicked on the page, `handleEmailClick()`\
   ``- copies the template form from the script tags, \
   \- inserts the correct email recipient to a hidden field, \
   \- inserts all this onto the page and displays the contact us form, and\
   \- in the background makes an ajax request to `/rest/email_token/create,` generating and saving a unique "session" token in the form.
4. When the submit button is clicked, `handleFormSubmit()` validates the form, and then submits, along with an authorization token to `/rest/email_session/contactform`.

{% hint style="info" %}
The `PostmarkAPI.php` in the `bos_email` module provides a "guaranteed delivery" type service.  It tries to send the email via the postMark API, and if it fails for some reason, queues the email for later delivery in a Drupal queue.\
Drupal will retry the email until it is able to send it to the postMark service.
{% endhint %}

{% hint style="info" %}
Email tracking with Drupal has been discontinued with the use of the PostMark service.  All logging can be obtained from the PostMark UI/Console.

Emails which fail to send can be viewed in the email\_contactform queue.
{% endhint %}

## Notes

### Sender email address

{% hint style="danger" %}
**This requirement could be obselete, and a requirement from earler versions of the form.**  \
****_Can consider removing this "feature" and reverting to having the sender be the email address provided by constituent.  That way the cob employee/recipient can simply reply to the email._
{% endhint %}

Emails are sent from an email address that is generated for each email sent. The format of the email address is:

{random\_string}@contactform.boston.gov

**We don't send the email from the original sender's email address as that could be a vector for an email spoofing attack.**

The reply\_to header of the email is set to the constituents email first and the unique address of the contact form second. When someone replies to the email the to address of the email is set to the constituents email first and the unique address second. This delivers two copies of the email. One goes directly to the constituent, and one to the contact form API. We log the response time using the copy that gets sent to the contact form API. Once the reply email is delivered to the constituent, further replies will be direct between the constituent and city.

### Process Workflow Diagram

[Diagram of the overall flow](https://user-images.githubusercontent.com/34665739/34566740-b4a90256-f12d-11e7-8162-f2abaaba7454.png) (needs updating)

### Future Enhancements

#### HTML Emails

The contact us email which is sent from postMark to the cob recipient is a plain text email.

The PostmarkAPI is capable of generating HTML emails.  \
A nicer experience for cob staff would be to receive an html email.

#### IP Locking

As well as the tokens etc, we could consider introducing an IP lock for say 60sec after a contact form submission is made (the timer should be managed on the server side, inside the endpoint). It should not affect genuine users but would minimize the impact (and success) of flood attacks and relay exploits on the endpoint.  If there were a rapid second submission from the same IPAddress, we would flash a warning back to the user to try again after 60 seconds.

#### Email Confirmations

At the moment, there is no confirmation of a successful submission other than the on-screen notification. We have the submitters' email address, so we could send an email confirming the submission (see next enhancement).

#### Sender Email Validation.

There is little in the way of validation of the email the submitter provides as a contact for responses. &#x20;

* The JavaScript validation process does checks that a "visually" valid email pattern is entered, (i.e. it is an email pattern string) but does not validate the email address exists.
* 2022 this ticket ([DIG-438](https://bostondoit.atlassian.net/browse/DIG-438)) added a second email address box to try to prevent typos in the email address.  While it helped a lot, it is not 100% effective and some emails from innocent errors and malicious actors (e.g. spammers) still slip through.
* 2023 [DIG-1675](https://bostondoit.atlassian.net/browse/DIG-1675) provided some modules which can check for DNS validity, disposable emails and also email blacklists. This would further help reduce invalid sender email addresses being provided.
* If we send email confirmations, then we could use that process to determine if the email address is active and does not have temporary errors (mailbox full etc).  Since we are using AJAX it would be simple to end the confirmation email, wait a period of time (say 10sec) and then query postMark to see if the email was delivered.  If it was then return success, if not then flag to the submitter on the form. This would not be 100% effective because some errors take time to be reported, and we cannot wait too long during validation.  The other two approaches above should still be implemented to try to filter out malicious actions, and to detect innocent errors before consuming email server resources.

