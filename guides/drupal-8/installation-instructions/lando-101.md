---
description: Using Lando in City of Boston.
---

# Lando 101

{% hint style="success" %}
**In the context of this document:** \
&#x20;  **   **_**Think of**_ [_**Lando**_](https://docs.lando.dev) _**as a**_ [_**CLI**_](https://en.wikipedia.org/wiki/Command-line\_interface) _**for a**_ [_**Drupal**_](https://en.wikipedia.org/wiki/Drupal) _****_ [_**LAMP**_](https://en.wikipedia.org/wiki/LAMP\_\(software\_bundle\)) _**stack constructed in**_ [_**docker**_](https://en.wikipedia.org/wiki/Docker\_\(software\))_**.**_
{% endhint %}

## What is Lando ?

For our purposes, Lando is a PHP-based tool-set which does 3 main things:

1. Provides a pre-packaged local docker-based development environment,
2. Provides a wrapper for common docker container management processes,
3. Provides a means to execute development commands inside the container from the host.&#x20;

## Lando 101.

### 1. A Pre-packaged Drupal infrastructure on your local computer.

Lando curates an appropriate LAMP stack for Drupal development, largely removing the need for this skill in the local development team.  The stack is contained within:

* Docker images that are maintained by Lando.
* A configuration file (landofile) which Lando parses into the necessary dockerfiles and utility scripts&#x20;

{% hint style="success" %}
COB uses a landofile which can be found at `/[reporoot]/.lando.yml`
{% endhint %}

### 2. Container Management

Lando provides a CLI for tasks developers commonly need to perform **on** the container.

{% hint style="info" %}
A full list of defined Lando commands can be obtained by executing: \
`lando`
{% endhint %}

| Command                                                                 | Explanation                                                                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ``[`lando start`](https://docs.lando.dev/basics/start.html)``           | Starts all 3 lando containers, building them if they don't already exist.                                                                                                                                                                                                                                                   |
| ``[`lando stop`](https://docs.lando.dev/basics/stop.html#usage)``       | Stops all 3 containers, but does not delete or destroy them.  They can simply be restarted later.                                                                                                                                                                                                                           |
| ``[`lando rebuild`](https://docs.lando.dev/basics/rebuild.html#usage)`` | <p>Will rebuild the container using the values in the <code>.lando.yml</code> and <code>.config.yml</code> files.</p><ul><li>If the containers have persistent images, these will be reused. </li><li>Any content in the database will be lost, </li><li>Project files cloned/managed by git will be left intact.</li></ul> |
| ``[`lando destroy`](https://docs.lando.dev/basics/destroy.html#usage)`` | <p></p><p>Will destroy the container.</p><ul><li>If the containers have persistent images, these will be retained. </li><li>Any content in the database will be lost, </li><li>Project files cloned/managed by git will be left intact.</li></ul>                                                                           |

### 3. Command Line Interface for tasks in the &#x20;

Lando provides a CLI for tasks developers commonly need to perform **in** the container.

| Command                                                         | Explanation                                                                                                                                                                                                                                                                                                                                                                          |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ``[`lando ssh`](https://docs.lando.dev/basics/ssh.html#usage)`` | <p>Opens a bash terminal on the appserver docker container. </p><p>If the -c switch is used, </p><p>  <code>lando ssh -c "&#x3C;command>"</code></p><p>then a terminal will be opened, the command provided will be run in the container and then the session will be closed.</p><p><code>eg:  lando ssh -c "ls -la /app/docroot/modules/custom"</code></p>                          |
| `lando drush`                                                   | <p>Executes a drush cli command in the appserver container: </p><p>  <code>lando drush &#x3C;command></code><br><code>eg lando drush status</code><br><em>Note: a drush alias can be passed like this:</em></p><p>  <code>lando drush @alias &#x3C;command></code></p><p><code>eg:  lando drush @bostond8.prod en dblog</code></p>                                                   |
| `lando drupal`                                                  | <p>Executes a Drupal cli command in the appserver container:</p><p>  <code>lando drupal &#x3C;command></code></p>                                                                                                                                                                                                                                                                    |
| `lando composer`                                                | <p>Executes a Composer command on the appserver container:</p><p><code>eg:  lando composer require drupal/paragraphs:^1.3</code></p>                                                                                                                                                                                                                                                 |
| `lando drupal-sync-db`                                          | <p>Executes a cob script which copies the database from the stage environment to the local development environment, and sync's all the configurations etc.</p><p>  <code>lando drupal-sync-db</code></p>                                                                                                                                                                             |
| `lando drupal-pull-repo`                                        | <p>Executes a cob script which pulls the latest project repository from gitHub and then clones and merges the private -repository.  Finally it runs sync tasks to update the DB with any new configurations.</p><p>  <code>lando drupal-pull-repo</code></p><p>To update the repo's without sync'ing the content, execute:</p><p>  <code>lando drupal-pull-repo --no-sync</code></p> |
| `lando validate`                                                | Locally runs the linting and PHPCode sniffing checks that are run by Travis.                                                                                                                                                                                                                                                                                                         |
| `lando switch-patterns`                                         | <p>Allows you to switch between patterns CDN hosts.</p><p>  <code>lando switch-patterns 2</code> switches to the local CDN in the patterns container</p><p>  <code>lando switch-patterns 3</code> switches to the production CDN</p><p>  <code>lando switch-patterns 4</code> switches to the stage patterns CDN.</p>                                                                |
|                                                                 |                                                                                                                                                                                                                                                                                                                                                                                      |

{% hint style="info" %}
A full list of defined Lando commands can be obtained by executing: \
`lando`
{% endhint %}

## Lando Workflows.

#### - Pull and sync latest version of the github for the checked-out branch.

&#x20; `lando drupal-pull-repo`

#### - Update local DB with content from Acquia Stage site

&#x20; `lando drupal-sync-db`

#### - Update both code and DB locally, but leave container intact

&#x20; `lando drupal-pull-repo --no-sync &&`\
&#x20; `lando drupal-sync-db`

#### - Completely rebuild local development environment

&#x20; `lando rebuild`\
or to be completely sure, run these commands from the \
&#x20; `lando destroy &&`\
&#x20;   `rm -rf <repo-path>`\
&#x20;   `git clone -b develop git@github.com:CityOfBoston/boston.gov-d8.git <repo-path>`\
&#x20;   `lando start`



``

