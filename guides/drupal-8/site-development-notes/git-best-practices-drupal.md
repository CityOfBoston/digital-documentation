# Git Best Practices - Drupal

## Boston.gov-d8 Repo Branching

City of Boston use Acquia to host our Drupal website.

Acquia provide a number of different environments for COB to use. One of those environments is **production** \(prod\) the others are non-production - named: stage, dev, uat, ci & dev2.

Detail on deployment is covered [elsewhere](../../../projects/decommissioned-apps-or-services/drupal-7/deployment-2019/), but in summary we are able to "bind" certain branches of our github repo \([CityofBoston/boston.gov-d8](https://github.com/CityOfBoston/boston.gov-d8)\) to these Acquia environments, and when changes occur in those branches, a deployment is automatically triggered.

Therefore, the way we branch off and push/merge into the main branches is important.

The `master` and `develop`branches are bound to the development and staging Acquia environments are used as the continuous deployment pipeline. _Changes cannot be made directly onto the `master`branch, and changes should not be made directly onto the `develop`branch, except when hotfixes are needed_.

Best Practice is to create _**working branches**_ off `develop` , then check those branches out locally. Once code is ready to be applied to repo, it should be committed and merged to the _working branch_ and then pushed to the `develop` branch in GitHub.

Branches attached to non-pipeline environments in Acquia are termed _**environment branches**_  \(see also [On-Demand Instances](../on-demand-instances/)\). These branches are created from the `develop`branch and attached to the environment. Developer then create their working branches off the environment branch and commit/push to that branch.  Once the project or piece of work is complete, the environment branch is merged down to the `develop` branch.

![Example Git Branch Usage](../../../.gitbook/assets/image%20%2827%29.png)

In the above diagram, 

* The `master` branch is the production branch and cannot be pushed to directly. The correct way to update it is to merge the `develop`branch into the `master` branch.  At all times the `master` branch should be a copy of the code on the production environment. \(see [continuous deployment](../continuous-deployment-process.md#deploy-to-staging-includes-automated-testing)\)
* Green arrows cause a deployment process \(controlled by Travis, taking approx 3 mins\).  
  * The code on the Acquia Environment is updated during the deploy.
* Orange arrows cause a build, test and deployment  process \(controlled by Travis, taking approx 30 mins\). 
  * The code on the Acquia Environment is updated during the deploy.
* Branches can be attached to multiple Acquia Environments, and deploys will occur in all environments when the repo branch is updated.

## Useful Links

{% page-ref page="../continuous-deployment-process.md" %}

{% page-ref page="../../../standards-and-best-practices/git-and-github/" %}

{% page-ref page="../on-demand-instances/" %}



