---
description: Standards we follow as developers.
---

# General

## Version control

* All new software is checked in to GitHub.
* Legacy software that needs maintenance gets moved to GitHub.
* Absent a _very_ compelling reason, repos are public from day one and source code is released under the [CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/) public domain license.

{% page-ref page="git-and-github/" %}

## Project structure

{% tabs %}
{% tab title="Web applications" %}
We’ve moved to a monorepo structure, and use [Lerna](https://lernajs.io) to manage it. This repository can be found at [https://github.com/CityOfBoston/digital/](https://github.com/CityOfBoston/digital/). `develop` is its main branch and is only updated by pull request/merge, **never** by committing directly.

All new webapps should be created under `/services-js`. Our internal modules are found at `/modules-js`. We are working on migrating older apps into the monorepo over time.
{% endtab %}

{% tab title="Drupal" %}

{% endtab %}
{% endtabs %}

## Local environment

{% tabs %}
{% tab title="Web applications" %}
* Use Yarn instead of NPM when installing packages, so we can take advantage of [workspaces](https://yarnpkg.com/lang/en/docs/workspaces/).
* Set up your editor to automatically run Prettier on save. See the guides: [https://prettier.io/docs/en/editors.html](https://prettier.io/docs/en/editors.html).
* Create Storybook stories to provide documentation for your components, and to allow for visual regression testing with Percy.
{% endtab %}

{% tab title="Drupal" %}
Set up automatic linting with PHPUnit \[???\]

DoIT use a set of PHPUnit driven linting tests to check coding standards are being met. Local Testing - Linting tests can be run locally by developers using lint utilities in their development environments. Developers can also execute a mirror of the tests run during CI \(by Travis\) using Phing tasks locally.
{% endtab %}
{% endtabs %}

## Subdomains

### Criteria

Boston.gov subdomains are assigned on a case-by-case basis. Typically, anything that is a core city service will qualify for a subdomain.

### **Naming Conventions**

If the website qualifies for a subdomain, it should adhere to the following conventions:

#### Specific to the purpose

* Example: [https://mayors24.cityofboston.gov/](https://mayors24.cityofboston.gov/) fulfills service requests for the Mayor’s 24

#### Descriptive of content on the site

* Example: [https://data.cityofboston.gov/](https://data.cityofboston.gov/) is the data hub for the City

#### Is not misleading/does not interfere with other City services

* [100% Talent: The Boston Women’s Compact](http://www.cityofboston.gov/women/talent/) does not use _talent.cityofboston.gov_ because “Talent” could be interpreted as “Employees”, thus misleading users who are searching for the [Career Center](http://www.cityofboston.gov/ohr/careercenter/).

