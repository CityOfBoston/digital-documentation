---
description: 'https://registry.boston.gov/birth'
---

# Birth certificates

## Historic Documentation

There were more constraints with this product than there were with death certificates. State law prohibits access to the records of individuals whose parents were not married when they were born. These _restricted_ records create a security and privacy challenge for us to tackle.

Also found here: [https://drive.google.com/drive/u/0/folders/14pKZCyDCDXWP32\_opAPOuog-DFbdxBCJ](https://drive.google.com/drive/u/0/folders/14pKZCyDCDXWP32_opAPOuog-DFbdxBCJ)

#### Where We Started

We focused on death certificates first because of the way the laws are structured in Massachusetts. Death certificates are entirely public record throughout the Commonwealth, so there are fewer privacy and security concerns associated with them.

The Registry Department has been working to digitize death certificates over the last few years, building out a database to be used for fulfillment. The database currently has records back to 1956.

We were able to leverage this database to connect it to a user-facing application on [Boston.gov](https://www.boston.gov/) which allows constituents to order death certificates online. Payment processing is routed through [Stripe](https://stripe.com/). The Registry receives the payment as well as the order request and can then ship a certified copy of the death certificate to the constituent by mail.

We successfully launched the death certificates application in March 2018 and have seen increases in online purchases \(versus in person or by mail\) since implementation. You can read more about the project [here](https://medium.com/innovation-and-technology/bringing-death-certificates-to-life-with-agile-tools-dcb0174cdc1a).

**Original Project Team**

* Product Management: Rachel Braun
* Engineering: Fiona \(Fin\) Hopkins, Jessica Marcus
* Design: Caroline Stjarnborg
* Database/Fulfillment: Rich Oliver, Scott Blackwell
* Subject Matter Expert: Patty McMahon \(City Registrar\) and Registry Team

Testing/staging environment: [https://registry-certs.digital-staging.boston.gov/birth](https://registry-certs.digital-staging.boston.gov/birth)

