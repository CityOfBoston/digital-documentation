---
description: Using Lando in City of Boston.
---

# Lando 101

{% hint style="success" %}
**In the context of this document:**   
   _**Think of**_ [_**Lando**_](https://docs.lando.dev/) _**as a**_ [_**CLI**_](https://en.wikipedia.org/wiki/Command-line_interface) _**for a**_ [_**Drupal**_](https://en.wikipedia.org/wiki/Drupal) _****_[_**LAMP**_](https://en.wikipedia.org/wiki/LAMP_%28software_bundle%29) _**stack constructed in**_ [_**docker**_](https://en.wikipedia.org/wiki/Docker_%28software%29)_**.**_
{% endhint %}

## What is Lando ?

For our purposes, Lando is a PHP-based tool-set which does 3 main things:

1. Provides a pre-packaged local docker-based development environment,
2. Provides a wrapper for common docker container management processes,
3. Provides a means to execute development commands inside the container from the host. 

## Lando 101.

### 1. A Pre-packaged Drupal infrastructure on your local computer.

Lando curates an appropriate LAMP stack for Drupal development, largely removing the need for this skill in the local development team.  The stack is contained within:

* Docker images that are maintained by Lando.
* A configuration file \(landofile\) which Lando parses into the necessary dockerfiles and utility scripts 

{% hint style="success" %}
COB uses a landofile which can be found at `/[reporoot]/.lando.yml`
{% endhint %}

### 2. Container Management

Lando provides a CLI for tasks developers commonly need to perform **on** the container.

{% hint style="info" %}
A full list of defined Lando commands can be obtained by executing:   
`lando`
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Command</th>
      <th style="text-align:left">Explanation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="https://docs.lando.dev/basics/start.html"><code>lando start</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Starts all 3 lando containers, building them if they don&apos;t already
        exist.</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="https://docs.lando.dev/basics/stop.html#usage"><code>lando stop</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Stops all 3 containers, but does not delete or destroy them. They can
        simply be restarted later.</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="https://docs.lando.dev/basics/rebuild.html#usage"><code>lando rebuild</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">
        <p>Will rebuild the container using the values in the <code>.lando.yml</code> and <code>.config.yml</code> files.</p>
        <ul>
          <li>If the containers have persistent images, these will be reused.</li>
          <li>Any content in the database will be lost,</li>
          <li>Project files cloned/managed by git will be left intact.</li>
        </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="https://docs.lando.dev/basics/destroy.html#usage"><code>lando destroy</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">
        <p></p>
        <p>Will destroy the container.</p>
        <ul>
          <li>If the containers have persistent images, these will be retained.</li>
          <li>Any content in the database will be lost,</li>
          <li>Project files cloned/managed by git will be left intact.</li>
        </ul>
        </td>
    </tr>
  </tbody>
</table>### 3. Command Line Interface for tasks in the  

Lando provides a CLI for tasks developers commonly need to perform **in** the container.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Command</th>
      <th style="text-align:left">Explanation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="https://docs.lando.dev/basics/ssh.html#usage"><code>lando ssh</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">
        <p>Opens a bash terminal on the appserver docker container.</p>
        <p>If the -c switch is used,</p>
        <p><code>  lando ssh -c &quot;&lt;command&gt;&quot;</code>
        </p>
        <p>then a terminal will be opened, the command provided will be run in the
          container and then the session will be closed.</p>
        <p><code>eg:  lando ssh -c &quot;ls -la /app/docroot/modules/custom&quot;</code>
        </p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>lando drush</code>
      </td>
      <td style="text-align:left">
        <p>Executes a drush cli command in the appserver container:</p>
        <p><code>  lando drush &lt;command&gt;<br />eg lando drush status</code>
          <br
          /><em>Note: a drush alias can be passed like this:</em>
        </p>
        <p><code>  lando drush @alias &lt;command&gt;</code>
        </p>
        <p><code>eg:  lando drush @bostond8.prod en dblog</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>lando drupal</code>
      </td>
      <td style="text-align:left">
        <p>Executes a Drupal cli command in the appserver container:</p>
        <p><code>  lando drupal &lt;command&gt;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>lando composer</code>
      </td>
      <td style="text-align:left">
        <p>Executes a Composer command on the appserver container:</p>
        <p><code>eg:  lando composer require drupal/paragraphs:^1.3</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>lando drupal-sync-db</code>
      </td>
      <td style="text-align:left">
        <p>Executes a cob script which copies the database from the stage environment
          to the local development environment, and sync&apos;s all the configurations
          etc.</p>
        <p><code>  lando drupal-sync-db</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>lando drupal-pull-repo</code>
      </td>
      <td style="text-align:left">
        <p>Executes a cob script which pulls the latest project repository from gitHub
          and then clones and merges the private -repository. Finally it runs sync
          tasks to update the DB with any new configurations.</p>
        <p><code>  lando drupal-pull-repo</code>
        </p>
        <p>To update the repo&apos;s without sync&apos;ing the content, execute:</p>
        <p><code>  lando drupal-pull-repo --no-sync</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
A full list of defined Lando commands can be obtained by executing:   
`lando`
{% endhint %}

## Lando Workflows.

#### - Pull and sync latest version of the github for the checked-out branch.

  `lando drupal-pull-repo`

#### - Update local DB with content from Acquia Stage site

  `lando drupal-sync-db`

#### - Update both code and DB locally, but leave container intact

  `lando drupal-pull-repo --no-sync &&  
  lando drupal-sync-db`

#### - Completely rebuild local development environment

  `lando rebuild`  
or to be completely sure, run these commands from the   
  `lando destroy &&  
    rm -rf <repo-path>  
    git clone -b develop git@github.com:CityOfBoston/boston.gov-d8.git <repo-path>  
    lando start`



\`\`



