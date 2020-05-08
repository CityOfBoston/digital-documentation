# Typical build output

The following is a printout of the console from a typical build following the instructions on Installation Instructions page.

Specifically this output is from the command:

```text
lando start
```

### Lando build log

```text
Let's get this party started! Starting app boston...
node version 8 is a legacy version! We recommend upgrading.
landoproxyhyperion5000gandalfedition_proxy_1 is up-to-date
Starting boston_patterns_1  ... done
Starting boston_appserver_1 ... done
Starting boston_database_1  ... done
Waiting until database service is ready...
Waiting until appserver service is ready...
Waiting until patterns service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...

ref: lando-drupal-post-start.sh
[LANDO] Project Event - post-start

================================================================================
[SUCCESS] Docker containers are now started.
================================================================================

     ____________      ______________________________________  
   ________________   |.H.H.H.H.H.H.H.H.H.H.H.H.H.H.H.H.H.H.H| 
 ____________________ |______________________________________| 
 ____       ___ _____   ||_~|~|~|~|~|~|~|~|~|~|~|~|~|~|~|_||   
|  _ \  ___|_ _|_   _|    |______________________________|     
| | | |/ _ \| |  | |       | HH |.H.H. Boston .H.H.| HH |      
| |_| | (_) | |  | |       | HH ||~|~|City Hall~|~|| HH |      
|____/ \___/___| |_|       | __ |__________________| __ |      
 ____________________      | HH |.H.H.H.'  '.H.H.H.| HH |      
   ________________        | HH ||~|~|~|!XX!|~|~|~|| HH |      
     ____________          |____|'''''''|~~|'''''''|____|      


[INFO] Host is MacOSX

   ___                      __        __        __     __        ______
  / _ )___  ___  __ _  ___ / /  ___ _/ /_____ _/ /__ _/ /_____ _/ / / /
 / _  / _ \/ _ \/  ' \(_-</ _ \/ _ `/  '_/ _ `/ / _ `/  '_/ _ `/_/_/_/ 
/____/\___/\___/_/_/_/___/_//_/\_,_/_/\_\\_,_/_/\_,_/_/\_\\_,_(_|_|_)  
                                                                       

Your app has started up correctly.
Here are some vitals:

 NAME            boston                                     
 LOCATION        /Users/stellaubaha/Documents/boston.gov-d8 
 SERVICES        appserver, database, patterns              
 APPSERVER URLS  https://localhost:32813                    
                 http://localhost:32814                     
                 http://boston.lndo.site/                   
                 https://boston.lndo.site/                  
 PATTERNS URLS   http://localhost:32815  
                 https://patterns.lndo.site/
```

### .lando.yml

The log above was generated using `lando start` with this`.lando.yml` [landofile](https://docs.lando.dev/config/lando.html#landofile).

```text
# Acquia friendly mods from https://github.com/lando/lando/issues/105
# See readme in ./build/readme.boston.md

name: boston
recipe: drupal8

config:
  php: '7.3'
  webroot: docroot
  database: mysql:5.7
  xdebug: true
  drupal: false

services:
  patterns:
    type: node:8
    overrides:
      environment:
        PORT: "3030"
        FRACTAL_PORT: "3030"
      labels:
        traefik.0.port: "3030"
        traefik.docker.network: "landoproxyhyperion5000gandalfedition_edge"
        traefik.0.frontend.rule: "HostRegexp:patterns.lndo.site"
    globals:
      gulp-cli: "latest"
    build_as_root:
      - printf "\n[LANDO] Node Container Event - build as root\n\n"
      - /app/scripts/local/lando-patterns-customize.sh
    run:
      - printf "\n[LANDO] Node Container Event - run\n\n"
      - /app/scripts/local/lando-build-patterns.sh
    command: /app/scripts/local/lando-patterns-post-start.sh

  database:
    type: mysql
    portforward: true
    host: localhost
    run_as_root:
      - printf "\n[LANDO] Database Event - run as root\n\n"
      - /app/scripts/local/lando-database-customize.sh

    creds:
      user: drupal
      password: drupal
      database: drupal

  appserver:
    type: php:7.3
    webroot: docroot
    xdebug: true
    overrides:
      environment:
        PHP_IDE_CONFIG: "serverName=boston.lndo.site"
        XDEBUG_CONFIG: "remote_enable=1 idekey=PHPSTORM"
    config:
      conf: xdebug.ini

    build_as_root:
      - printf "\n[LANDO] Appserver Event - build as root\n\n"
      - /app/scripts/local/lando-appserver-customize.sh

    build:
      - printf "\n[LANDO] Appserver Event - build\n\n"
      - /bin/sh -c "composer require drush/drush:^9 --no-update --no-progress --no-suggest --prefer-dist &> /dev/null"

    run:
      - printf "\n[LANDO] Appserver Event - run\n\n"
      - /app/scripts/local/lando-build-drupal.sh -y

tooling:
  phpunit:
    service: appserver
    description: "Run PHP Unit tests: lando phpunit"

  drush:
    service: appserver
    description: "Run drush commands in app container: lando drush <command>"
    cmd:
    - /app/vendor/bin/drush

  drupal:
    service: appserver
    description: "Run drupal-console commands in app container: lando drupal <command>"
    cmd:
    - drupal

  npm:
    service: patterns
    description: "Run npm commands in patterns:node container: lando npm <command>"
    cmd:
      - echo a; cd /app/patterns && npm

  node:
    service: patterns
    description: "Run node commands in patterns container: lando node <command>"

  gulp:
    service: patterns
    description: "Run node:gulp commands in patterns container: lando gulp <command>"

  mycli:
    service: database
    description: "Open mycli prompt in db container: lando mycli"
    cmd: "mycli -udrupal -pdrupal"

  drupal-sync-db:
    service: appserver
    description: "Drupal: Copy the remote staging DB into the local DB"
    cmd:
      - /app/scripts/local/sync.sh 'test'

  drupal-pull-repo:
    service: appserver
    description: "Drupal: Pull the latest Drupal public and private repos from Git"
    cmd:
      - /app/scripts/local/pull.sh

  validate:
    service: appserver
    description: "Run the Linting and PHPCS routines on the current branch."
    cmd:
      - /app/scripts/local/validate.sh all

  switch-patterns:
    service: appserver
    description: "Switch the patterns library: 2 = local, 3 = prod, 4 = stage."
    cmd:
      - /app/vendor/bin/drush drush bcss

events:
  post-start:
    - appserver: /app/scripts/local/lando-drupal-post-start.sh

```

### .config.yml

The log above was generated using `lando start` with this`config.yml` project file.

```text
project:
  docroot: ${REPO_ROOT}/${lando_config_webroot}
  profile:
    name: bos_profile
  themes:
    - bos_theme
  php:
    memory_size: 1024M

# Drupal Account Credentials. These are used for installing Drupal.
drupal:
  account:
    name: admin
    password: admin
    mail: no-reply@acquia.com
  multisite:
    # Use 'default' as the name if there are no multisites.
    name: default

patterns:
  local:
    build: true
    repo:
      name: CityOfBoston/patterns.git
      branch: develop
      # Where the repo will be cloned to.
      local_dir: ${REPO_ROOT}/patterns
  travis:
    build: false

build:
  local:
    # Build Type = dev or prod.
    type: dev
    config:
      # Define the folder, relative to the drupal docroot, for config files to be synchronised, no trailing slash.
      # aquia requires ./config/default:
      #     @see https://docs.acquia.com/acquia-cloud/develop/config-d8/
      sync: "../config/default"
    database:
      # Set source to 'initialize' to start a fresh install.
      # Otherwise set it to 'sync' (and provide a drush-alias) to sync from the drush-alias environment.
      # Tip: Develop environment has database in closest state to the github repo configs.
      source: sync
      drush_alias: "@bostond8.dev"
      host: boston_database_1
      port: 3306

  travis:
    develop:
      # With Travis, the type will control what type of build is deployed to Acquia (dev/prod/none).
      # none = don't alter modules enabled from configuration
      type: dev
      suppress_output: 0
      database:
        # Set source to 'initialize' to start a fresh install.
        # Otherwise set it to 'sync' to sync from the drush-alias environment.
        source: initialize
        drush_alias: "@bostond8.dev"
      config:
        # Set to 'true' or 'false'.  False means no configs will be imported during build/deploy
        # Note: if the commit is a hotfix, then configs wont be imported for expediency.
        sync: "true"
    master:
      # With Travis, the type will control what type of build is deployed to Acquia (dev/prod).
      # none = don't alter modules enabled from configuration
      type: prod
      suppress_output: 0
      database:
        # Set source to 'initialize' to start a fresh install.
        # Otherwise set it to 'sync' to sync from the drush-alias environment.
        source: initialize
        drush_alias: "@bostond8.test"
      config:
        # Set to 'true' or 'false'.  False means no configs will be imported during build/deploy
        # Note: if the commit is a hotfix, then configs wont be imported for expediency.
        sync: "true"

deploy:
  # Each element of this deploy node is a branch in the main CoB repo.
  # WARNING, scripts cannot track branches with spaces or dashes ("-") in their name.  AVOID and use _ instead.
  #          If your branchname must have a space or "-", then in this file just remove them (i.e. my-branch = mybranch)

  develop:
    # Folder in Travis container where deploy files will be "built"
    dir: ${REPO_ROOT}/deploy
    # Name of the target branch in the Acquia repo. Should be quoted if it contains - or _ or spaces.
    deploy_branch: "develop-deploy"
    # Path to the drush command in the Travis container.
    travis_drush_path: '${REPO_ROOT}/vendor/bin/drush'
    # Alias for deploy target Aquia server.
    drush_alias: "@bostond8.dev"
    # Definition of files that will and wont be copied from build to deploy.
    includes_file: ${REPO_ROOT}/scripts/deploy/deploy-includes.txt
    excludes_file: ${REPO_ROOT}/scripts/deploy/deploy-excludes.txt
    sanitize_file: ${REPO_ROOT}/scripts/deploy/deploy-sanitize-files.txt
    # Dry-run (for testing).
    dry_run: false
    # Whether (and where) to sync the database on the deploy target. NB: copy-db=false means db left intact.
    copy_db: false
    drush_db_source: "@bostond8.test"
    # user:host for (Acquia) git remote to be used for deployment.
    acquia_repo: bostond8@svn-29892.prod.hosting.acquia.com:bostond8.git
    
  master:
    # Folder in Travis container where deploy files will be "built"
    dir: ${REPO_ROOT}/deploy
    # Name of the target branch in the Acquia repo. Should be quoted if it contains - or _ or spaces.
    deploy_branch: "master-deploy"
    # Path to the drush command in the Travis container.
    travis_drush_path: '${REPO_ROOT}/vendor/bin/drush'
    # Alias for deploy target Aquia server.
    drush_alias: "@bostond8.test"
    # Definition of files that will and wont be copied from build to deploy.
    includes_file: ${REPO_ROOT}/scripts/deploy/deploy-includes.txt
    excludes_file: ${REPO_ROOT}/scripts/deploy/deploy-excludes.txt
    sanitize_file: ${REPO_ROOT}/scripts/deploy/deploy-sanitize-files.txt
    # Dry-run (for testing).
    dry_run: false
    # Whether (and where) to sync the database on the deploy target. NB: copy-db=false means db left intact.
    copy_db: false
    drush_db_source: "@bostond8.prod"
    # user:host for (Acquia) git remote to be used for deployment.
    acquia_repo: bostond8@svn-29892.prod.hosting.acquia.com:bostond8.git

# Configuration settings for new git repository.
git:
  # [Optional] Set this node (private_repo) if there is a private repo that needs to
  # be included as part of the build:
  # - It is expected that the repo is hosted on GitHub.
  # - The development machine *must* have a valid ssh key for this remote repo.
  # - The repo can contain a settings file (e.g. private.settings.php)
  # - The folder structure of the private repo must exactly match the folder structure
  #   of the main repo so that files can be copied across to the correct folders.
  private_repo:
    repo: CityOfBoston/boston.gov-d8-private.git
    branch: develop
    deploy_branch: develop
    # Where the repo will be cloned to.
    local_dir: ${LANDO_MOUNT}/setup/private
    # Provide the path to a settings script (relative to the private repo root),
    # which will be copied to the main repo and then set up to be called from (i.e. extend)
    # the sites main settings.php script.
    settings_file: sites/default/settings/private.settings.php

composer:
  bin: ${LANDO_MOUNT}/vendor/bin

drush:
  bin: ${LANDO_MOUNT}/vendor/bin/drush
  cmd: ${LANDO_MOUNT}/vendor/bin/drush  -r ${LANDO_MOUNT}/docroot -l default
  root: ${LANDO_MOUNT}/docroot

```

