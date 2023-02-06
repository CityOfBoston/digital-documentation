---
description: >-
  Not relevant for active project, but for historical information/reference.
  Last updated July 2018.
---

# Archived Forms Information

We're working to provide a digital form alongside all paper forms and pdfs in the City of Boston. If you want to come to City Hall to do business with us, we'll be here for you. But, if your only option is to come to a City office to do business with us, then we can do better.

#### Project History

When we [City of Boston Digital Team](https://boston.gov/) started our project to digitize 'all forms', we didn't have a list of what forms residents could fill out online. We had to start by scraping the City's old website \(cityofboston.gov\) for all pdf files. That, plus interviews with departments and reviews of other documentation helped us pull together an initial list. Even with months of auditing, we're still finding new forms every day.

Early in the process, we went through a bid process for a pilot program for a web form tool. We solicited three bids and procured a partner, [SeamlessDocs](https://www.seamlessdocs.com/). Our goal in the pilot was to move fast and learn. We weren’t trying to fix every department’s process but instead to focus on the customer experience and digitize paper forms and pdfs.

## Things we learned from our pilot

* There is huge demand to move forms online in the departments.
* We initially thought there would be a strong demand for submissions that look exactly like current paper forms. That hasn't been the case.
* Departments almost immediately asked for tools to help them move to all-digital processes;
* We've avoided getting into workflow changes, but there is a growing need for a workflow tool;
* Functionality around conditional fields, logic, and branching are really important;

## Where we are going

We've learned that not all forms are created equal. Some are simple, but most of them are either interrelated or kick off complicated business processes. Just moving those online isn't much help. Some are really also better served as applications. A good example is death certificates. After learning more about the process, we realized that a [form](https://github.com/CityOfBoston/Forms/issues/230) isn't a good solution since we wanted to give residents the ability to look up and order a certificate once they knew we had it \(Right now, they request a record blind\). So we are building [an entire application](https://github.com/CityOfBoston/registry-certs/) that allows residents to look up and order death certificates.

As we expand, we'll need both a tool for building forms and a flexible workflow tool into which information from simple forms and web applications can be fed. We've done some analysis, and are working on an RFP around that. Since that's a bigger project, we're currenctly coordinating among multiple stakeholders to release the RFP.

However, the pilot has also shown us we'll always need the ability to quickly and easily create branded forms.

#### Links

* [Forms Repository](https://github.com/CityOfBoston/Forms)
* [Project tracking in Github projects](https://github.com/orgs/CityOfBoston/projects/5)

#### Document

* [Bid Document From Pilot With SeamlessDocs](https://github.com/CityOfBoston/Forms/blob/master/3.9.16%20Request%20for%20Written%20Quotes%20-%20Forms%20Tool.pdf)

#### [Priorities/Forms in Process](https://github.com/orgs/CityOfBoston/projects/5)

Known bugs/how to resolve: Hello, I attempting to customize submission email notifications. I want to have a 'full name' field show up in the subject of the email but the only field \(out of a handful of fields\) available for me to map is a single line input field. Any thoughts? 5:51 pm December 22

Hey there, is the Full Name field assigned to a specific signer?  sorry what do you mean

Sometimes if fields are assigned to a specific signer, they can't be pulled into the email subject.  But actually are you working with a SeamlessDocs \(PDF\) or Web Form? Could you send me a link to your form? [https://boston.seamlessdocs.com/forms/builder/CO17121000048706650](https://boston.seamlessdocs.com/forms/builder/CO17121000048706650) They shouldn't be assigned to a specific signer we want whatever applicant put in their name to appear in the subject line

I see. Try using a Single Line field for Full Name, instead of the Full Name field. That should work. 

