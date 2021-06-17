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

## Git Branches

For Drupal see:

{% page-ref page="../../guides/drupal-8/site-development-notes/git-best-practices-drupal.md" %}

