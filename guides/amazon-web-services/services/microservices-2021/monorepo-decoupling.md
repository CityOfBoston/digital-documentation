---
description: Work Estimation
---

# Project: Monorepo Decoupling

The goal is to separate each web app/service into their own repository so that the code and deploy process for each is isolated and independent.&#x20;



The core component of the monorepo are the following:

* Common Libraries (modules-js directory)
  * Each directory here creates an npm-like module that can be imported into each of the WebApp, as needed
  * These apps should be isolated, moved into their own repositories and create proper modules that can either be imported in some manner into the projects that needed.
  * If we create npm-like modules from these common pieces of code we can set version numbers for each minor/major updates, allowing us to pin-down WebApps so certain versions if they can go any further
* Deploy Code
  * Take the current deploy code and replace it with the new process created to go through AWS's CodeCommit
* Web Apps (services-js directory)



![](<../../../../.gitbook/assets/COB - MonoRepo Decoupling.jpeg>)



