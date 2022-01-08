# Contact Form

Matt actively updating how the contact form works.

--

The contact form app does three things:

* Send email to [feedback@boston.gov](mailto:feedback@boston.gov) \(and other addresses\) from forms on Boston.gov and other sites.
* Record times it takes to send responses to feedback emails via a webhook from the email sender.
* Handle newsletter subscription requests by making API calls to Upaknee.

It has no public UI and serves just as an AJAX endpoint \(via CORS\) for Boston.gov HTML forms.

It has a volume and response time dashboard behind a password, as well as the Sidekiq dashboard.

### Sending email from Boston.gov

The contact form API sends emails to any linked email address on Boston.gov. The API uses [Postmark](https://postmarkapp.com/) to handle the sending of the emails.

#### Logging

When an email is sent using a contact form on Boston.gov, the API logs the IP address of the sender along with the message, to address, name, and from address in a database.

#### Sending

Emails are sent from an email address that is generated for each email sent. The format of the email address is:

{random\_string}@contactform.boston.gov

**We don't send the email from the original sender's email address as that could be a vector for an email spoofing attack.**

The reply\_to header of the email is set to the constituents email first and the unique address of the contact form second. When someone replies to the email the to address of the email is set to the constituents email first and the unique address second. This delivers two copies of the email. One goes directly to the constituent, and one to the contact form API. We log the response time using the copy that gets sent to the contact form API. Once the reply email is delivered to the constituent, further replies will be direct between the constituent and city.

[Diagram of the overall flow](https://user-images.githubusercontent.com/34665739/34566740-b4a90256-f12d-11e7-8162-f2abaaba7454.png)

#### **Production Links**: [https://contactform.boston.gov/](https://contactform.boston.gov/) – [Sidekiq dashboard](https://contactform.boston.gov/sidekiq)

#### Documentation: [Upaknee re-subscribe API](https://drive.google.com/file/d/1DS7MlR7ayjy6lCV5ZKZ29zOkux2sqcRc/view)

#### Potential Future Enhancements \[which will be cleared out quarterly based on public feedback/needs: [Add ability to send to separate email addresses within a department \(conditional sending\)](https://github.com/CityOfBoston/boston.gov/issues/696)

