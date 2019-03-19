---
description: Deployment practice and workflows from March 2019.
---

# Deployment \(2019\)

### Deployment step-by-step Workflow.

The following is a table showing the various stages of a deploy to production.

{% tabs %}
{% tab title="Develop" %}
* Developer checks out the `develop` branch of the main repository.
* Developer builds local docker container, and builds Drupal site in container.
* Developer creates new working branch e.g. `my-branch`.
* Developer makes necessary changes to website and/or to PHP code.
* Developer tests changes locally.

{% hint style="info" %}
There are scripts which can be used to make the container and build Drupal.
{% endhint %}
{% endtab %}

{% tab title="Commit" %}
{% hint style="warning" %}
Important - don't forget ....

* Developer updates features \(using Drupal features module\) as needed.
{% endhint %}

* Developer commits code and features changes to `my-branch` branch of local repository.
* Develop runs local PHPUnit, behat and linting tests.
* Developer pushes local branch `my-branch` to a branch of the same name \(i.e. `my-branch`\) on the [CoB GitHub repository](https://github.com/CityOfBoston/boston.gov-d7).
{% endtab %}

{% tab title="Release" %}
* Developer creates a new **Pull Request** \(PR\) to merge `my-branch` into `develop`on the [boston.gov-d7](https://github.com/CityOfBoston/boston.gov-d7) GitHub repository.
  * Developer provides appropriate notes \(in template form\) in the PR comments.
  * Developer assigns a person to review the code.

{% hint style="success" %}
Once a new PR to `develop` is created in GitHub, Travis starts a build verification process, which attempts to build a new Drupal site from the files `my-branch`, and then run various linting, PHPUnit and Behat tests.
{% endhint %}

* Developer merges`my-branch`into `develop` when the peer-review is complete and the Travis build tests pass. 
* Developer deletes `my-branch`  locally and on GitHub when the merge to `develop` is complete.

{% hint style="success" %}
**It is acceptable to use the GitHub "Squash and Merge" function when merging a branch into `develop`.**
{% endhint %}

{% hint style="info" %}
Travis monitors the GitHub`develop` branch, and when a commit is performed \(either a direct commit or a merge process\) to the branch Travis starts a deploy process:

* [ ] The deploy process:
  * [ ] re-runs the build tests, 
  * [ ] clones the `develop-build` branch from an Acquia managed git repository,
  * [ ] copies the built website files \(which are from the `develop` branch of the GitHub repository\) from the Travis container into the cloned Acquia branch,
  * [ ] and then commits the  to the `develop-build` branch to the Acquia repository.
{% endhint %}

{% hint style="success" %}
The `dev`Acquia server/environment monitors the Acquia `develop-build`branch and when that branch is updated \(i.e. a merge/commit is made\) it automatically pulls the updated code onto the appropriate server, and 

* backs up the database on the Acquia `dev` environment
* copies the database from the Acquia `stage` environment to the Acquia `dev`environment.
* runs processes on the `dev` environment to sync the \(updated\) code and the \(copied\) database.
{% endhint %}
{% endtab %}

{% tab title="Deploy to stage\(QA\)" %}
* A Developer creates a PR to merge `develop`into `master` when a deploy is desired.
* A developer merges the PR into `master`.

{% hint style="danger" %}
**Do not use GitHub's "Squash and Merge" feature when merging the PR as this breaks consistency between `master` and `develop` on GitHub.**
{% endhint %}

{% hint style="info" %}
Travis monitors the GitHub `stage` branch, and when a commit is performed \(either a direct commit or a merge process\) to the branch Travis starts a deploy process:

* [ ] The deploy process:
  * [ ] re-runs the build tests, 
  * [ ] clones the `master-build` branch from an Acquia managed git repository,
  * [ ] copies the built website from the Travis container over files in the cloned branch,
  * [ ] and then commits the  `master-build` branch to the Acquia repository.
* [ ] The Acquia server/environment monitors the Acquia repository

   branch and when it is updated \(a commit is made\) pulls the updated code onto the appropriate server.
{% endhint %}

{% hint style="success" %}
The `stage`Acquia server/environment monitors the Acquia `master-build`branch and when that branch is updated \(i.e. a merge/commit is made\) it automatically pulls the updated code onto the appropriate server, and 

* backs up the database on the Acquia `stage` environment, and
* copies the database from the Acquia `prod` environment to the Acquia `stage`environment, and
* runs processes on the `stage` environment to sync the \(updated\) code and the \(copied\) database.
{% endhint %}
{% endtab %}

{% tab title="Deploy to prod \(Live\)" %}
In the Acquia Cloud web-UI.

* A developer drags the code from the `stage` environment to the `prod` environment.

{% hint style="success" %}
Acquia-hooks will detect that the code has been moved and will:

* backup the `Production` database,
* run drush commands to update configurations from the new code into the `Production` database.
{% endhint %}
{% endtab %}
{% endtabs %}

==========================================================================

### Deployment Engineering.

The automated deploy process follows continuous deploy \(CD\) principles whereby:

* The deploy workflow is engineered so that all developers are able to performing a deployment,
* The workflow is automated to remove the need for manual tasks and testing wherever possible.

The primary tools used by City of Boston in the CD workflow process are:

* **Docker** to manage local _development_ environments.
* **GitHub** for _code storage_ and _deploy initiation_.
* **Travis** for _automated testing, building_ and _packaging_.
* **Acquia Cloud** \(acapi and cloud webhooks\) for _deployment_.

Secondary tools used by City of Boston in the CD process are:

* **Phing** to abstract scripting processes used in build, test and packaging.
* **PHPUnit** and **behat** to perform automated testing.

Overall the engineering workflow is as follows:

```text
Deploy Worflow Start
 ├── Github used to deploy to Acquia dev environment
    └── developers test
 ├── Github used to deploy to Acquia stage environment
    └── QA test
 ├── Acquia web UI used to deploy to Acquia prod environment
 └── deploy complete
```

```text
Deploy to Acquia Environment
├── Git commit to GitHub branch
  ├── Travis application triggered from GitHub
    ├── Drupal build new artifact in remote Travis container
    ├── automated testing in remote Travis container
    ├── build artifact commited to Git in Travis
    └── Travis Git branch commited to Acquia Git
  ├── Acquia cloud triggered from Acquia Git
    ├── Acquia Cloud hooks triggered
      ├── Code copied into Acquia environment
      ├── Drupal DB backup 
      ├── new DB copy pulled from next environment in workflow 
      └── DB and code synchronized
```

### www.boston.gov and hub.boston.gov

{% hint style="info" %}
City of Boston operate 2 websites \(hub and boston\), each being an "application" on Acquia.  The Drupal code-base on the 2 websites is very similar, the main differences being some settings files and the content contained in the applications database.

The overall engineering utilizes a single repo in GitHub, and a single repo in Acquia's Git.  

The workflow engineering creates and manages a single `develop` and single `master` branch in GitHub -these ultimately are common to hub and boston websites.   
However, the Acquia environments \(dev, stage, prod\) in each application \(boston, hub\) are attached to different branches of the Acquia git. Hub branches are suffixed with `-hub`. These branches are created by and during the Travis packaging process.
{% endhint %}

