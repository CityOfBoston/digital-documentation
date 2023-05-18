---
description: The automated tests we run in our projects.
---

# Automated tests & static analysis

## Local dev environment

{% tabs %}
{% tab title="Drupal 8" %}
DoIT uses a set of PHPUnit driven linting tests to check coding standards are being met.

Linting tests can be run locally by developers using lint utilities in their development environments. Developers can also execute a mirror of the tests run during CI (by Travis) using Phing tasks locally.
{% endtab %}

{% tab title="Web applications" %}
* Jest
* Storybook
* ESLint
* Prettier
{% endtab %}
{% endtabs %}

## Continuous integration

{% tabs %}
{% tab title="Drupal 8" %}
As part of the code acceptance in the CI workflow, Travis runs linting tests via Phing. Code changes cannot be pulled and merged into development branches until these tests are passed.
{% endtab %}

{% tab title="Web applications" %}
### Travis

* Unless a repo is static content or an app with a very limited lifespan (_e.g._ private Amazon bid site) it must have some tests, which must be easily runnable by Travis.
* Test failures on the default branch must be addressed immediately.
* In general, tests must pass before merging a PR, though if the change will not affect the tests (such as a documentation or script change) it’s okay to override that requirement so you can move on.
* Travis should handle all packaging for deployment for maximum repeatability. For security reasons, it’s generally best to have a second process for actually executing the deploy, however.
* Flaky tests are not tolerated.

### Percy.io

* All apps with UI should be set up to submit to Percy.io as part of every PR.
* For React-based apps, this typically means using Storybook and is a good reason to make state-lite UI components that are fully exercised by Storybook.
* The Percy.io configuration should be set for a mobile width, the smallest desktop width, and a large desktop width. (See “Testing Breakpoints” above.)
* Percy.io changes must be approved before merge. Make sure there are no surprises.
* Non-deterministic UI, such as Mapbox or a random icon, should be either made deterministic or removed when run by Percy. Flaky UI is not tolerated.

### BrowserStack

We use BrowserStack to live test our website and mobile apps across mobile devices and platforms. BrowserStack’s device cloud carries an exhaustive list of devices that lets us easily/speedily switch between multiple breakpoints to test our site’s responsiveness.

We are currently experimenting with [TestCafe](https://testcafe.devexpress.com/) to run browser integration tests on top of BrowserStack.
{% endtab %}
{% endtabs %}
