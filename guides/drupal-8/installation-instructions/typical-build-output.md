# Typical build output

The following is a printout of the console from a typical build following the instructions on Installation Instructions page.

Specifically this output is from the command:

```text
lando start
```

### Lando build log

```text
Let's get this party started! Starting app..
landoproxyhyperion5000gandalfedition_proxy_1 is up-to-date
Creating network "boston_default" with the default driver
Creating volume "boston_data_appserver" with default driver
Creating volume "boston_home_appserver" with default driver
Creating volume "boston_data_database" with default driver
Creating volume "boston_home_database" with default driver
Creating volume "boston_data_node" with default driver
Creating volume "boston_home_node" with default driver
Creating boston_node_1 ... done
Killing boston_node_1 ... done
Creating boston_appserver_1 ... done
Killing boston_appserver_1 ... done
Starting boston_appserver_1 ... done
Killing boston_appserver_1 ... done
Starting boston_appserver_1 ... done

[LANDO] Appserver Event - build as root

Killing boston_appserver_1 ... done
Starting boston_appserver_1 ... done

[INFO] Installing Linux packages in appserver container. 
[INFO] During build, container build actions will be logged to files in /app/setup 
     - After build, log files can be accessed from https://boston.lndo.site/sites/default/files/setup/ 
[SUCCESS] Docker container 'appserver' is built.
 
Killing boston_appserver_1 ... done
Starting boston_appserver_1 ... done
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   599    0   599    0     0   2036      0 --:--:-- --:--:-- --:--:--  2037
100 4580k  100 4580k    0     0   816k      0  0:00:05  0:00:05 --:--:--  948k
Drush Commandline Tool 9.7.1
Killing boston_appserver_1 ... done
Starting boston_appserver_1 ... done

[LANDO] Appserver Event - build

Killing boston_appserver_1 ... done
Starting boston_appserver_1 ... done
Killing boston_appserver_1 ... done
Starting boston_node_1 ... done
/var/www/.npm-global/bin/gulp -> /var/www/.npm-global/lib/node_modules/gulp-cli/bin/gulp.js
+ gulp-cli@2.2.0
added 237 packages from 156 contributors in 8.983s
Killing boston_node_1 ... done
Starting boston_appserver_1 ... done
Starting boston_node_1      ... done
Creating boston_database_1  ... done
Waiting until database service is ready...
Waiting until appserver service is ready...
Waiting until node service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...
Waiting until database service is ready...

[LANDO] Appserver Event - run

========================================================================================= 
[INFO] Installing Drupal and dependencies in appserver & database containers. 
=========================================================================================
 
[INFO] see /app/setup/composer.log for output. - (or https://boston.lndo.site/sites/default/files/setup/composer.log)
[INFO] Executes: > composer install --prefer-dist --no-suggest --no-interaction 
[SUCCESS] Composer has loaded Drupal core, contrib modules and third-party packages/libraries.
 
[INFO] Clone and merge private repo into main project repo. 
[SUCCESS] Repo merged.
 
[INFO] Update settings files. 
[SUCCESS] Settings updated.
 
==== Installing Drupal =========== 
[INFO] see /app/setup/drush_site_install.log for output. - (or https://boston.lndo.site/sites/default/files/setup/drush_site_install.log)
[INFO] SYNC Mode: Will copy remote DB and then import repo configs. 
[INFO] Copying database (and content) from @bostond8.test into docker database container. 
 [notice] Starting to dump database on source.
 [notice] Starting to discover temporary files directory on target.
 [notice] Copying dump file from source to target.
 [notice] Starting to import dump file onto target database.
[SUCCESS] Site has database and content from remote environment.
 
[INFO] Import configuration from sync folder: '/app/docroot/../config/default' into database - This may take some time ...
       - -> follow along at /app/setup/config_import.log (or https://boston.lndo.site/sites/default/files/setup/config_import.log
[SUCCESS] Config from the repo has been applied to the database.
 
[INFO] Enable/disable appropriate development features and functionality. -  This may also take some time ...
 [action] Enable DEVELOPMENT-ONLY modules.
 [action] Enable and set stage_file_proxy.
 [action] Disable acquia connector and purge.
 [action] Disable prod-only and unwanted modules.
[SUCCESS] Development environment set.
 
[INFO] Apply pending database updates etc. 
 [success] No pending updates.
[SUCCESS] Done.
 
[INFO] Update the drush configuration and aliases. 
[SUCCESS] Drush file updated.
 
[SUCCESS] Drupal build finished. - 
Appserver Drupal install/build took  14 minutes and  40 seconds.


[LANDO] Database Event - run as root

[INFO] Install MySQl tools (mycli and pspg) into the database container. 
OK
[SUCCESS] Installed mycli and pspg.
 

[LANDO] Project Event - post-start

================================================================================
[SUCCESS] - Appserver and MySQL Docker containers are now started.
------------------------------------------------------------------------------------

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


The admin account password is reset to: admin.
http://default/user/reset/1/1576514209/bZfPbdXRoGuVkdx7KeKL1pvUZQfxD1B_ZTsJZz069PA/login

BOOMSHAKALAKA!!!

Your app has started up correctly.
Here are some vitals:

 NAME            boston                    
 LOCATION        /home/david/sources/test  
 SERVICES        appserver, database, node 
 APPSERVER URLS  https://localhost:33302   
                 http://localhost:33303    
                 http://boston.lndo.site   
                 https://boston.lndo.site  

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
  node:
    type: node:10
    globals:
      gulp-cli: "latest"

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
        PHP_IDE_CONFIG: "serverName=bostontest.lndo.site"
        XDEBUG_CONFIG: "remote_enable=1 remote_host=10.0.0.74 idekey=PHPSTORM"
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
    service: node
    description: "Run npm commands in node container: lando npm <command>"

  node:
    service: node
    description: "Run node commands in node container: lando node <command>"

  gulp:
    service: node
    description: "Run node:gulp commands in node container: lando gulp <command>"

  phing:
    service: appserver
    description: "Run Phing tasks in app container: lando phing <task>"
    cmd:
      - /app/vendor/bin/phing

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

events:
  post-start:
    - printf "\n[LANDO] Project Event - post-start\n\n"
    - printf "================================================================================\n"
    - printf "\033[1;32m[SUCCESS] - Appserver and MySQL Docker containers are now started.\033[0m\n"
    - printf "------------------------------------------------------------------------------------\n\n"
    - cat ${LANDO_MOUNT}/setup/uli.log && rm -f ${LANDO_MOUNT}/setup/uli.log
```

### .config.yml

The log above was generated using `lando start` with this`config.yml` project file.

```text
project:
  docroot: ${LANDO_MOUNT}/${lando_config_webroot}
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
      drush_alias: "@bostond8.test"
      host: boston_database_1
      port: 3306

  travis:
    develop:
      # With Travis, the type will control what type of build is deployed to Acquia (dev/prod).
      type: dev
      database:
        # Set source to 'initialize' to start a fresh install.
        # Otherwise set it to sync to sync from the drush-alias environment.
        source: initialize
        drush_alias: "@bostond8.dev"
      config:
        # define the folder, relative to the drupal docroot, for config files to be synchronised, no trailing slash.
        # aquia require ./config/default:
        #     @see https://docs.acquia.com/acquia-cloud/develop/config-d8/
        sync: "../config/default"
    master:
      # With Travis, the type will control what type of build is deployed to Acquia (dev/prod).
      type: prod
      database:
        # Set source to 'initialize' to start a fresh install.
        # Otherwise set it to sync to sync from the drush-alias environment.
        source: initialize
        drush_alias: "@bostond8.test"
      config:
        # define the folder, relative to the drupal docroot, for config files to be synchronised, no trailing slash.
        # aquia require ./config/default:
        #     @see https://docs.acquia.com/acquia-cloud/develop/config-d8/
        sync: "../config/default"

deploy:
  # each element of the deploy object is a branch in the main CoB repo.
  develop:
    # Folder in Travis container where deploy files will be "built"
    dir: ${LANDO_MOUNT}/deploy
    # Name of the target branch in the Acquia repo.
    deploy_branch: develop-deploy
    # Path to the drush command in the Travis container.
    travis_drush_path: '${LANDO_MOUNT}/vendor/bin/drush'
    # Alias for deploy target Aquia server.
    drush_alias: "@bostond8.dev"
    # Definition of files that will and wont be copied from build to deploy.
    includes_file: ${LANDO_MOUNT}/scripts/deploy/deploy-includes.txt
    excludes_file: ${LANDO_MOUNT}/scripts/deploy/deploy-excludes.txt
    sanitize_file: ${LANDO_MOUNT}/scripts/deploy/deploy-sanitize-files.txt
    # Message we want to appear in Acquia repo for the deployment commit.
    commit_message: "Automated commit by Travis CI for Build \#\${travis.id}"
    # Dry-run (for testing).
    dry_run: false
    # Whether (and where) to sync the database on the deploy target. NB: copy-db=false means db left intact.
    copy_db: false
    drush_db_source: "@bostond8.stage"
    # Array of user:host for (Acquia) git remotes to be used for deployment.
    remotes:
      - bostond8@svn-29892.prod.hosting.acquia.com:bostond8.git
  master:
    # Folder in Travis container where deploy files will be "built"
    dir: ${LANDO_MOUNT}/deploy
    # Name of the target branch in the Acquia repo.
    deploy_branch: master-deploy
    # Path to the drush command in the Travis container.
    travis_drush_path: '${LANDO_MOUNT}/vendor/bin/drush'
    # Alias for deploy target Aquia server.
    drush_alias: "@bostond8.test"
    # Definition of files that will and wont be copied from build to deploy.
    includes_file: ${LANDO_MOUNT}/scripts/deploy/deploy-includes.txt
    excludes_file: ${LANDO_MOUNT}/scripts/deploy/deploy-excludes.txt
    sanitize_file: ${LANDO_MOUNT}/scripts/deploy/deploy-sanitize-files.txt
    # Message we want to appear in Acquia repo for the deployment commit.
    commit_message: "Automated commit by Travis CI for Build \#\${travis.id}"
    # Dry-run (for testing).
    dry_run: false
    # Whether (and where) to sync the database on the deploy target. NB: copy-db=false means db left intact.
    copy_db: false
    drush_db_source: "@bostond8.prod"
    # Array of user:host for (Acquia) git remotes to be used for deployment.
    remotes:
      - bostond8@svn-29892.prod.hosting.acquia.com:bostond8.git

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

