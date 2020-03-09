# Ruby

Contact Form

This application receives the content and headers from our contact form \(email\) [component](https://patterns.boston.gov/components/detail/contact_form.html), stores the data and queues it for delivery to our mail client \([postmark](https://postmarkapp.com/developer)\). This app is hosted on our AWS as an ECS container along with other Services-JS apps. Every instance/place that uses the contact form component is

