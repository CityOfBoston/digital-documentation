# Git Best Practices - Drupal

## Boston.gov-d8 Repo Branching

City of Boston use Acquia to host our Drupal website.

Acquia provide a number of different environments for COB to use. One of those environments is **production** \(prod\) the others are non-production - named: stage, dev, uat, ci & dev2.

Detail on deployment is covered [elsewhere](../continuous-deployment-process.md), but in summary we are able to "bind" certain branches of our github repo \([CityofBoston/boston.gov-d8](https://github.com/CityOfBoston/boston.gov-d8)\) to these Acquia environments, and when changes occur in those branches, a deployment is automatically triggered.

Therefore, the way we branch off and push/merge into the "bound" branches is important.

The `master` and `develop`branches are bound to the development and staging Acquia environments are used as the continuous deployment pipeline. _Changes cannot be made directly onto the `master`branch, and changes should not be made directly onto the `develop`branch, except when hotfixes are needed_.

Best Practice is to create _**working branches**_ off `develop` , then check those branches out locally. Once code is ready to be applied to repo, it should be committed and merged to the _working branch_ and then pushed to the `develop` branch in GitHub.

Branches attached to non-pipeline environments in Acquia are termed _**environment branches**_  \(see also [On-Demand Instances](../on-demand-instances/)\). These branches are created from the `develop`branch and attached to the environment. Developer then create their working branches off the environment branch and commit/push to that branch.  Once the project or piece of work is complete, the environment branch is merged down to the `develop` branch.

![Example Git Branch Usage](../../../.gitbook/assets/image%20%2827%29.png)

In the above diagram, 

* Lines with an arrow indicate a merge \(and/or push\) to the branch in the direction of the arrow.
* Lines with a dot connector indicate the creation or updating of branch, and when to a local branch a pull to that local branch.
* The `master` branch is the production branch and cannot be pushed to directly. The correct way to update it is to merge the `develop`branch into the `master` branch.  At all times the `master` branch should be a copy of the code on the production environment. \(see [continuous deployment](../continuous-deployment-process.md#deploy-to-staging-includes-automated-testing)\)
* Green arrows cause a deployment process:
  * Only if the branch being merged into is bound to an acquia environment, and 
  * This is controlled/executed  by Travis, taking approx 3 mins \(uses 30 Travis credits\), and
  * The website hosted on the Acquia Environment is updated during the deploy.
* Orange arrows cause a build, test and deployment process:
  * Only if the branch being merged into is bound to an acquia environment, and
  * This is controlled/executed by Travis, taking approx 30 mins \(uses 300 Travis credits\), and
  * The website hosted on the Acquia Environment is updated during the deploy.
    * Travis is configured so that this is extended process usually only runs when committing to the `develop` branch - triggering a deploy to the Acquia Dev environment as the first step of the deployment pipeline.
* Black arrows indicate a simple commit/merge process with no building or deploying:
  * Best practice reuquires that "working branches" are never bound to Acquia Environments, so
  * Travis is not involved, so uses 0 Travis credits
* **Note:** Any github branch can be bound to **one or more** Acquia Environments, and deploys will occur to all bound environments when the github branch is updated.
  * Travis always controls deploys and only one set of credits is used per branch update regardless of how many Acquia environments are bound to it.

## Useful Links

{% page-ref page="../continuous-deployment-process.md" %}

{% page-ref page="../../../standards-and-best-practices/git-and-github/" %}

{% page-ref page="../on-demand-instances/" %}



