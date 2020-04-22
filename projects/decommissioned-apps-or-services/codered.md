---
description: Historical emergency alert sign up - moved to Everbridge April 2020
---

# CodeRed

**Notes as of 1/16/18 \(plus relevant edits\)**

Takes form data from a component on Boston.gov \(example here: [https://www.boston.gov/departments/emergency-management\#sign-up-for-alerts](https://www.boston.gov/departments/emergency-management#sign-up-for-alerts)\) and and processes it with the CodeRed API.

[CodeRed](https://www.onsolve.com/solutions/products/codered/?nabe=4761028343889920:0&utm_referrer=https%3A%2F%2Fwww.google.com%2F) is a software used by the City to send emergency notifications. People need to opt into the system.

We have a proxy application that sits in the middle, because CodeRed/Alert Boston requires you to login every time before interacting with it, the front end UI of Alert Boston does not match up with the City's brand, and we can not easily integrate/embed the Alert Boston/CodeRed UI into boston.gov: [https://public.coderedweb.com/cne/en-US/BFB5F355FAB8](https://public.coderedweb.com/cne/en-US/BFB5F355FAB8) \(this does the same thing as the form on boston.gov, but also allows for multilingual sign up\)

Contacts \(no longer with City\):  
~~Product: Josh Gee  
Engineering: Matt Crist, Fin Hopkins~~

Documents:

* [CG Import Guide.pdf](https://github.com/CityOfBoston/boston.gov/files/1635669/CG.Import.Guide.pdf)
* [CodeRED Web API for Contact Groups v3.0 \(1\).pdf](https://github.com/CityOfBoston/boston.gov/files/1635670/CodeRED.Web.API.for.Contact.Groups.v3.0.1.pdf)
* [ECN FTP Contact Group Instructions.pdf](https://github.com/CityOfBoston/boston.gov/files/1635671/ECN.FTP.Contact.Group.Instructions.pdf)
* [Example template for sending contacts](https://github.com/CityOfBoston/boston.gov/files/1391197/CGtemplate_language.2.xlsx)

