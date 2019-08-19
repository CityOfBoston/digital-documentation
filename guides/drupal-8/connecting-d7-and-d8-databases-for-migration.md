---
description: >-
  Run a direct MySQL database migration from a D7 local lando to a D8 local
  lando.
---

# Connecting D7 and D8 databases for migration

1. Spin up the D7 environment in lando \(`lando start`, then `lando build-bos`\) and spin up the D8 environment \(`lando start`\) separately. 

2. Go to your local [portainer](https://www.portainer.io/) \([http://portainer.lndo.site:9090](http://portainer.lndo.site:9090) sign in with admin, admin\). Then click on "Containers &gt; bosd7\_database\_1", scroll to the bottom and join network `boston_default`. 

![](../../.gitbook/assets/join.png)

3. Make sure your `docroot/sites/default/settings/settings.local.php` on D8 has the information it needs to connect to D7:

```text
$databases['migrate']['default'] = [
  'driver' => 'mysql',
  'database' => 'drupal',
  'username' => 'drupal',
  'password' => 'drupal',
  'host' => 'bosd7_database_1',
  'port' => '3306',
];
```

4. Test your connection by checking the migration status in D8 `lando drush ms`

If `lando drush ms` has a blank output, try running the following:

```text
lando drush cim --partial --source=modules/custom/bos_migration/config/install/ -y; and lando drush cr; and lando drush ms
```

For more info, checkout: [https://www.drupal.org/docs/8/api/migrate-api/migrate-source-plugins/migrating-data-from-a-sql-source](https://www.drupal.org/docs/8/api/migrate-api/migrate-source-plugins/migrating-data-from-a-sql-source)

