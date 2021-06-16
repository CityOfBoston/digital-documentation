---
description: Practices and workflows for using Git and GitHub on the Digital team.
---

# Git / GitHub

## General

* Changes are made on feature branches and merged via Pull Request, regardless of whether those PRs are reviewed.
* Preferred naming convention for a feature branch is `service-name/feature`.
* **Never** commit directly to `develop` or `production` branches.
* Rebase to a single commit before making a PR.
* Commit messages should be written in present tense.

## Pull Requests

{% hint style="info" %}
See our [code review](https://boston.gitbook.io/digital-documentation/standards-and-best-practices/code-reviews) guidelines.
{% endhint %}

* PRs should be merged by the assignee, who is typically the person who made the PR.
* Assignee should delete the branch once it has been merged into `develop`.
* Code reviews are required if more than one engineer has interest in the repo, regardless of the relative experience between the engineers.
* Code reviews take priority over feature work, and must be responded to quickly.
* Reviewer is responsible for approving changes detected by Percy.

## Branching

City of Boston use Acquia to host our Drupal website.

Acquia provide a number of different environments for us to use, one of those environments is **production** \(prod\) the others are named, but all are non-production.

Detail on deployment is covered elsewhere, but in summary we are able to "bind" certain branches of our github repo \([CityofBoston/boston.gov-d8](https://github.com/CityOfBoston/boston.gov-d8)\) to these Acquia environments, and when changes occur in those branches, a deployment is automatically triggered.

Therefore, the way we branch off and push/merge into the main branches is important.

The `master` and `develop`branches are bound to the development and staging Acquia environments are used as the continuous deployment pipeline. _Changes cannot be made directly onto the `master`branch, and changes should not be made directly onto the `develop`branch, except when hotfixes are needed_.

Best Practice is to create _**working branches**_ off `develop` , then check those branches out locally. Once code is ready to be applied to repo, it should be committed and merged to the _working branch_ and then pushed to the `develop` branch in GitHub.

Branches attached to non-pipeline environments in Acquia are termed _**environment branches**_. These branches are created from the `develop`branch and attached to the environment. Developer then create their working branches off the environment branch and commit/push to that branch.  Once the project or piece of work is complete, the environment branch is merged down to the `develop` branch.

![](../../.gitbook/assets/image%20%2826%29.png)

