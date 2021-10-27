---
description: >-
  City of Boston strives to automate the develop, test, package and deploy
  process at each step from local development to deployment in live production,
---

# Continuous Deployment Process

{% hint style="danger" %}
This page is out of date and needs review (as at 17 June 2021)
{% endhint %}

### Local Build

{% tabs %}
{% tab title="Entry Critera" %}
#### Clean development environment.

The repository is cloned in a local folder and ready for building.

This entry condition can be achieved:&#x20;

1. If you have not yet built the boston.gov website on your local machine, or
2. If you have cloned a new branch or created a new branch that you wish to build, you can run the `doit rebuild quick` script, or
3. If you have the repository cloned, but wish to delete it and rebuild a fresh website from a branch on the GitHub repository, you can run `doit rebuild full <branch>.` If you don't specify a branch, then `develop` will be used.

{% hint style="info" %}
Local developer responsible for creating local development environment.
{% endhint %}
{% endtab %}

{% tab title="Process used" %}
{% hint style="info" %}
The local build process is defined and controlled by Lando when `lando start` is executed.
{% endhint %}

{% hint style="info" %}
The `doit` scripts serve to prepare the cloned repository prior to running `lando start`
{% endhint %}

#### Local Build Steps.

**Lando**\
****`lando start` causes the following processes to be run from lando.yml

1. 3 standard Linux (ubuntu) containers are created.  One optimized as an appserver with Apache, one optimized as a database server with MySQL and one with Node. &#x20;
2. Install the required/dependent packages and tools -including Phing and Composer.&#x20;
3. Create and install XDebug and other Apache/PHP settings files.
4. Set apache vhosts and container's network configs. (done by Docker via Lando).
5. Start all 3 containers.
6. Launch the phing script `setup:docker:drupal-local`.

**Phing** \
The phing script`setup:docker:drupal-local` in `reporoot/scripts/phing/tasks/setup.xml`executes the following:

1. Download Drupal dependencies into Apache appserver container - including Drush. (done using Composer).
2. Download confidential settings and copy into Drupal file system (using Git).
3. Install Drupal by Installing a new Database on the database container. (using Drush).
4. Install Drupal modules and load configuration files. (using Drush).
5. Run Drupal's Update process to load updated-settings from modules. (using Drush).
6. Modify Drupal settings with localized settings.
7. Reset the admin password and issue login url. (using Drush).
{% endtab %}

{% tab title="Exit Criteria" %}
#### A fully built and functioning website in a docker container.

{% hint style="info" %}
There are 3 docker containers, proxy, appserver and database.
{% endhint %}

{% hint style="info" %}
Depending on configuration for the build, the database is either built locally as a new database (has no content), or is synchronised from a remote location (has content).
{% endhint %}
{% endtab %}
{% endtabs %}

### Local Development

{% tabs %}
{% tab title="Entry Criteria" %}
#### Repository Code and appropriate database in docker containers on local machine, ready for local development.

{% hint style="info" %}
Local developer responsible for verifying local development environment and verifying build and establishing the correct code start-point for development.
{% endhint %}
{% endtab %}

{% tab title="Process Used" %}

{% endtab %}

{% tab title="Exit Criteria" %}
#### Branch containing local code commits checked into GitHub repo and Pull Request created, peer approved and has passed automated testing. &#x20;
{% endtab %}
{% endtabs %}

### Deploy to Develop (includes automated testing)

{% tabs %}
{% tab title="Entry Criteria" %}
#### Pull Request in GitHub which is ready to merge into develop branch on GitHub.

The merge will (re)Run an automated build and testing process, and if successful deploy the develop branch to the Acquia develop environment.

{% hint style="info" %}
Development team responsible for ready decision.
{% endhint %}
{% endtab %}

{% tab title="Process Used" %}


1. Run Linting Test using PHP Linting. (done by PHP via Phing, launched by Travis).
2. Run Code Sniffer Test. (done by Sqizzlabs via Phing, launched by Travis).
3. (coming soon) Run Behat behavioral tests. (done by Behat via Phing, launched by Travis).
4. (coming soon) Run PHPUnit functional tests. (done by PHPUnit via Phing, launched by Travis).

### **Notes**

For local development, the docker container build is controlled by Lando, with Phing being used to build Drupal.

When a Pull Request is created to merge code into the develop branch on Github a test build and some automated testing is run by Travis. Travis is used in place of Lando to initiate and control the build process as described above (i.e. Travis is used  to build docker containers on Github/Travis infrastructure, whereas Lando builds docker containers on local machines).  In both cases the Travis and Lando scripts are very similar in structure and identical (as possible) in function.  Once the containers a built, both tools use the same Phing scripts to build and initiate Drupal.

(coming soon) Terraform will be used to spin up on-demand test/develop/experiment/demo instances of the containers (i.e. the websites) on AWS infrastructure. In this case Terraform scripts will be used to control the build in place of Lando - but (as with Travis) will be as similar as possible in function. Again, once the containers a built on AWS, the same Phing scripts will be used to build Drupal.
{% endtab %}

{% tab title="Exit Criteria" %}
#### Code from GitHub develop branch (and copy of production database) installed and functional on Acquia development environment.
{% endtab %}
{% endtabs %}

### Deploy to Staging (includes automated testing)

{% tabs %}
{% tab title="Entry Criteria" %}
#### Code base in GitHub develop branch is ready for deployment to Acquia Staging environment.

{% hint style="info" %}
Development team responsible for ready decision.
{% endhint %}
{% endtab %}

{% tab title="Process Used" %}


1. Run Linting Test using PHP Linting. (done by PHP via Phing, launched by Travis).
2. Run Code Sniffer Test. (done by Sqizzlabs via Phing, launched by Travis).
3. (coming soon) Run Behat behavioral tests. (done by Behat via Phing, launched by Travis).
4. (coming soon) Run PHPUnit functional tests. (done by PHPUnit via Phing, launched by Travis).

### **Notes**

For local development, the docker container build is controlled by Lando, with Phing being used to build Drupal.

When a Pull Request is created to merge code into the develop branch on Github a test build and some automated testing is run by Travis. Travis is used in place of Lando to initiate and control the build process as described above (i.e. Travis is used  to build docker containers on Github/Travis infrastructure, whereas Lando builds docker containers on local machines).  In both cases the Travis and Lando scripts are very similar in structure and identical (as possible) in function.  Once the containers a built, both tools use the same Phing scripts to build and initiate Drupal.

(coming soon) Terraform will be used to spin up on-demand test/develop/experiment/demo instances of the containers (i.e. the websites) on AWS infrastructure. In this case Terraform scripts will be used to control the build in place of Lando - but (as with Travis) will be as similar as possible in function. Again, once the containers a built on AWS, the same Phing scripts will be used to build Drupal.
{% endtab %}

{% tab title="Exit Criteria" %}
#### Code from GitHub master branch (and copy of production database) installed and functional on Acquia staging environment.
{% endtab %}
{% endtabs %}

### &#x20;QA

{% tabs %}
{% tab title="Entry Criteria" %}
#### GitHub master branch code and copy of production DB is on Acquia Staging environment.

{% hint style="info" %}
Development team responsible for ready decision.
{% endhint %}
{% endtab %}

{% tab title="Process Used" %}

{% endtab %}

{% tab title="Exit Criteria" %}
#### GitHub master branch code on staging environment is ready for deploy to production.
{% endtab %}
{% endtabs %}

### Deploy to Production

{% tabs %}
{% tab title="Entry Criteria" %}
#### Code base in GitHub master branch is ready for production deploy to Acquia production environment.

{% hint style="info" %}
QA team responsible for ready decision
{% endhint %}

{% hint style="warning" %}
Change Management must be advised of intent to deploy prior to deploy.
{% endhint %}
{% endtab %}

{% tab title="Process Used" %}

{% endtab %}

{% tab title="Exit Criteria" %}
#### Code from master branch is installed and functional, with the production database preserved and functional on Acquia production environment.
{% endtab %}
{% endtabs %}

