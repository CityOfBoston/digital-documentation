# Registry-Certs

### Application Structure

This App is designed to process `City of Boston` requests of birth, death, marriage certificates, and marriage intention applications. The Birth, Death, and Marriage requests are paid requests that get processed through the`Stripe` payment processing platform. Marriage Intention requests currently don't make use of our `Stripe` processing because at the moment their payments are being handled elsewhere. However, they share about 70-80% the same functionality as the other request types in the WebApp.

The entry point for the Registry Apps is `pages/_app.tsx`. This is a React.js app that uses the MobX library for state management. The entry point uses the `*CertificateRequest` classes as `PageDependencies` in each of the applications/request forms. These `*CertificateRequest` classes define the data fields and methods the application/forms will use. `*CertificateRequests` have a few `key` methods that serialize and the application `state` in the browser session among other things.

App Structure

* `Applicatiton Controller` \[`pages/_app.tsx`\]
  * \[`client/store/`\]
    * BirthCertificateRequest`(Class)`
    * DeathCertificateRequest`(Class)`
    * MarriageCentificateRequest`(Class)`
    * MarriageIntentionRequest`(Class)`

#### Application Controller

The application controller exist a few methods to be aware of such as a `routerlistener`, `screenreaderSupport`, `siteAnalytics`, `orderProvider`, etc. These are general use methods that will be inherited in almost all child applications. Of these, `siteAnalitics` and `orderProvider` are the most important. `siteAnalytics` explains itself, however, `orderProvider` is used only by the certificate requests that use `Stripe` payment. `orderProvider` handles storing and manipulating the required order information like shipping address, credit card info, etc. within the `sessionStorage`. When the Application Controller is mounted, `*CertificateRequests` fields are merged with the field data from the `orderProvider`.

