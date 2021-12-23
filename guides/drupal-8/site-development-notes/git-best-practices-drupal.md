# Git Best Practices - Drupal

## Boston.gov-d8 Repo Branching

City of Boston use Acquia to host our Drupal website.

Acquia provide a number of different environments for COB to use. One of those environments is **production** the others are non-production - named: **stage**, **dev**, **uat**, **ci** & **dev2**.

Detail on deployment is covered [elsewhere](../continuous-deployment-process.md), but in summary we are able to "bind" certain branches of our GitHub repo ([CityofBoston/boston.gov-d8](https://github.com/CityOfBoston/boston.gov-d8)) to these Acquia environments, and when changes occur in those branches, a deployment is automatically triggered.

Therefore, the way we branch-off, push-to and merge the "bound" branches is important.

### Normal Deployment Pipeline

The `develop` branch is bound to the Acquia **dev** environment, and the `master` branch to the **stage** environment. _Changes cannot be made directly onto the `master`branch, and changes should not be made directly onto the `develop`branch - except when hotfixes are needed_.

* Best Practice is to create a _**working branch**_ off `develop` , then check out that `working branch` locally.&#x20;
* Updated code should be committed to the locally checked out copy of the `working branch`&#x20;
  * Updating the local `working branch` will update the local containerized website for testing.
* Periodically, the local `working branch` should be pushed to the remote `working branch` in GitHub_._&#x20;
  * Updating the `working branch` in GitHub will not trigger any deploys or update any website. __&#x20;
* To start the deploy to the **dev** environment,  a PR is created in GitHub to merge the `working branch` in GitHub into __ the `develop` branch in GitHub.&#x20;
  * Merging will trigger a build and the website on the **dev** environment will be updated.
* When ready to deploy to the **stage** environment,  a PR is created in GitHub to merge the `develop` __ into __ the `master` branch in GitHub.&#x20;
  * Merging will trigger a build and the website on the **stage** environment will be updated.
* To deploy to the **production** environment, use the Acquia Cloud UI - see [continuous deployment](../continuous-deployment-process.md#deploy-to-production) notes.

### On-Demand Pipeline

We can bind a branch to the **dev2, ci** or **uat** environments so that we can share proposed or interim website changes with stakeholders or other individuals where a local containerized website is not appropriate.  These environments can be considered on-demand, and the way to update them is similar but slightly to the normal deploy piepline, requiring an extra branch.

Branches attached to environments other than **dev**, **stage** and **production** in Acquia are termed _**environment branches**_  (see also [On-Demand Instances](../on-demand-instances/)).&#x20;

* Initially, an `environment branch` is created from the `develop`branch.&#x20;
  * This `environment branch` is then bound to the desired Acquia environment (**dev2**, **ci** or **uat**).&#x20;
* Developers then create a `working branch` off the `environment branch` and check out that `working branch`locally. &#x20;
* Developers commit their work to the local copy of the `working branch` which can be pushed to the remote `working branch` in GitHub whenever desired.&#x20;
  * Updating the local `working branch` will update the local containerized website for testing.
  * Updating the `working branch` in GitHub will not trigger any deploys or update any website. &#x20;
* When ready to update the website on the bound environment, using a PR, the GitHub copy of the `working branch` is merged to the `environment branch` in GitHub.&#x20;
  * Merging will trigger a deploy to the bound Acquia environment (i.e. **dev2**, **uat** or **ci**) and update the website on that environment. &#x20;
  * Stakeholders can be directed to the website on the Acquia environment.
* Once the project or piece of work is complete, a PR to merge the GitHub`environment branch` to the `develop` branch is created.&#x20;
  * Merging will trigger a deploy to **dev** and update the website.
* To continue to deploy to **stage** and **production** environments, follow the notes in Normal Deploy Pipeline above.

### Branch Diagram

Sometimes a picture is worth 1,000 words.

![Example Git Branch Usage](<../../../.gitbook/assets/image (27).png>)

In the above diagram,&#x20;

* Lines with an arrow indicate a merge to the branch in the direction of the arrow.
* Lines with a dot connector indicate the creation (or updating) of a branch - and when the line is to a local branch it is a checkout to a local branch.
* The `master` branch is the **production** branch and cannot be pushed/merged to directly.&#x20;
  * The correct way to update `master` is to merge the `develop`branch into the `master` branch. &#x20;
  * At all times the `master` branch should be a copy of the code on the production environment. (see [continuous deployment](../continuous-deployment-process.md#deploy-to-staging-includes-automated-testing))
* Green arrows cause a deployment process:
  * Only if the branch being merged into is bound to an Acquia environment, and&#x20;
  * This is controlled/executed  by Travis, taking approx 3 mins (uses 30 Travis credits), and
  * The website hosted on the Acquia Environment is updated during the deploy.
* Orange arrows cause a build, test and deployment process:
  * Only if the branch being merged into is bound to an Acquia environment, and
  * This is controlled/executed by Travis, taking approx 30 mins (uses 300 Travis credits), and
  * The website hosted on the Acquia Environment is updated during the deploy.
    * Travis is configured so that this is extended process usually only runs when committing to the `develop` branch - triggering a deploy to the Acquia Dev environment as the first step of the deployment pipeline.
* Black arrows indicate a simple commit/merge process with no building or deploying:
  * Best practice reuquires that a `working branch` is not bound to Acquia Environments
  * Merging does not trigger Travis, there is no deploy and 0 Travis credits are used

****

* **Note:** A GitHub `environment branch` can be bound to **one or more** Acquia Environments. When this is the case, deploys will occur simultaeously to all bound environments when the GitHub `environment branch` is updated.
  * Travis always controls deploys, but only one set of credits is used per `environment branch` merge regardless of how many Acquia environments it is bound to.

## Useful Links

{% content-ref url="../continuous-deployment-process.md" %}
[continuous-deployment-process.md](../continuous-deployment-process.md)
{% endcontent-ref %}

{% content-ref url="../../../standards-and-best-practices/git-and-github/" %}
[git-and-github](../../../standards-and-best-practices/git-and-github/)
{% endcontent-ref %}

{% content-ref url="../on-demand-instances/" %}
[on-demand-instances](../on-demand-instances/)
{% endcontent-ref %}

