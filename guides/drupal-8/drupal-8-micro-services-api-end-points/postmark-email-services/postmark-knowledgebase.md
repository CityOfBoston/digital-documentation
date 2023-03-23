# Postmark Knowledgebase

<details>

<summary>Useful Links</summary>

* [City of Boston Postmark Account](https://account.postmarkapp.com/servers)
* [Postmark Email API](https://postmarkapp.com/developer/api/email-api)
* [Postmark Sandboxes](https://postmarkapp.com/developer/user-guide/sandbox-mode)
* [Testing Bounces in Sandboxes](https://postmarkapp.com/support/article/1239-how-to-test-bounces)
* [Drupal Management Page for Postmark](https://content.boston.gov/admin/config/system/boston)



</details>

<details>

<summary>Email Sender (From / from_address).</summary>

The Postmark API deployed by CoB requires that the sender be someone from the **boston.gov** domain (i.e. someone@boston.gov). This is a measure to minimize the likelihood of using the service as an unauthorized relay.

However, this means that we cannot use another domain, e.g. someone@gmail.com as the SMTP From address.

Instead, we can use the **ReplyTo** feature of SMTP which is supported by PostMark and will cause "replies" to the email to go to the ReplyTo address

_Both From and ReplyTo fields accept the format `Full Name<person@email.com>`_ _and the recipients email software should then show Full Name instead of person@email.com._

</details>

<details>

<summary>Email Bouncing and Recipient Suppression</summary>

When an email for a recipient receives a Hard Bounce the recipient is automatically added to a Suppression list and **will receive no further emails until the recipient email is removed from the list.**

</details>

<details>

<summary>Server Suspension / Disabling</summary>

If the email delivery to bounce and non-delivery ratio drops below a certain level, Postmark will automatically suspend an email Server.  No further emails will be sent from this Server.

A Server can also be suspended when a threshold of spam reports is reached.

This is to protect the Servers confidence rating and to prevent blacklisting by external services.

Watch for emails from Postmark advising when this happens.

Sandboxes do not have this restriction, they are never automatically suspended.

</details>

<details>

<summary>Use of Postmark Sandboxes and Servers</summary>

Postmark have a concept of servers which a user can set up to handle inbound and outbound email messages.

**Definitions:**

**Servers** are fully functioning objects which are "live" and have various **Streams** which send and receive email messages. A server can also have user-defined **Templates** and and **API Token** so mail can be routed via the server using an API. Streams actively monitor email messages sent and received and react to traffic based on rules which can be configured by the user.

**Sandboxes** are like Servers which are configured for testing. They have all the same features as a Server except that their Streams are "black holes" which do not actually send out email messages.  Sandboxes can generate failure messages when reserved email addresses are used as the recipient, otherwise all emails they receive are considered to be delivered (even though they aren't physically sent).

**CoB Use:**

City of Boston use a number of Servers to send Production-originating outbound emails, typically one server per originating source or application.

City of Boston have configured **one** Server for Testing where the sending of emails is an important part of the functionality being tested.  Templates can be added to this server as needed. To avoid accidental delivery of emails and possible suspension of the Server for non-delivery etc, this Server should only be used sparingly.

City of Boston use one sandbox for testing outbound emails.  All non-production environments which dont actually need emails to be delivered should use the Test Sandbox rather than the Test Server.



</details>
