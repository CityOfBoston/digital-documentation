---
description: Create a functional drupal site on AWS.
---

# AWS Setup Drupal

## Create a Database

{% embed url="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/php-hadrupal-tutorial.html\#php-hadrupal-tutorial-database" %}

{% tabs %}
{% tab title="Develop" %}
| Template | Dev/Test | MySQL 5.7.22 |  |
| :--- | :--- | :--- | :--- |
| Region | us-east-2c |  |  |
| DB Instance | thehubdev |  |  |
| DB Password | stIFuLFRED | s16788 |  |
| Size | db.m5.large |  |  |
| Storage | SSD 100GB | autoscale 1000GB + No Standby |  |
| Connectivity | public: no | existing VPC |  |
| Authentication | password |  |  |
| Subnets | [subnet-996cc2d4](https://us-east-2.console.aws.amazon.com/vpc/home?region=us-east-2#subnets:filter=subnet-996cc2d4) | [subnet-73bfd808](https://us-east-2.console.aws.amazon.com/vpc/home?region=us-east-2#subnets:filter=subnet-73bfd808) | [subnet-c02d08a9](https://us-east-2.console.aws.amazon.com/vpc/home?region=us-east-2#subnets:filter=subnet-c02d08a9) |
| Subnet group | vpc-7f411416 |  |  |
| VPC Security Groups | default \(sg-bf21d9d7\) |  |  |
| Endpoint | [thehubdev.ct1hwaaivjk3.us-east-2.rds.amazonaws.com](http://thehubdev.ct1hwaaivjk3.us-east-2.rds.amazonaws.com) |  |  |
{% endtab %}

{% tab title="Prod" %}

{% endtab %}
{% endtabs %}

## Create VPC with necessary components

Make sure IAM account `username+cli` is a member of `elasticbean_dev` group.

Install elasticbeanstalk CLI from [https://github.com/aws/aws-elastic-beanstalk-cli-setup](https://github.com/aws/aws-elastic-beanstalk-cli-setup) use the readme for instructions.  At the end, from \(a **bash shell**\) be sure to add to the path using:

`echo 'export PATH="/home/david/.pyenv/versions/3.7.2/bin:$PATH"' >> /home/david/.bash_profile && source /home/david/.bash_profile`

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)

## Code commit

make sure iam `username+cli` account is member of  `codepipeline` group.



#### Resources

| Description | Link |
| :--- | :--- |
| eb command line reference | [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-cmd-commands.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-cmd-commands.html) |
| Overview | [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/php-hadrupal-tutorial.html\#php-hadrupal-tutorial-database](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/php-hadrupal-tutorial.html#php-hadrupal-tutorial-database) |
|  |  |

## sync data from Acquia

rsync -arz -essh -P thehub.prod@thehub.ssh.prod.acquia-sites.com:/var/www/html/thehubdev/docroot/sites/default/files/ ~/sources/aws-codecommit/hub-files/

rsync -arz -essh -P thehub.dev@thehubdev.ssh.prod.acquia-sites.com:/var/www/html/thehubdev/acquia-files/nobackup/ ~/sources/aws-codecommit/cob-drupal-hub/nobackup/

rsync -arz -essh -P thehub.prod@thehub.ssh.prod.acquia-sites.com:/var/www/site-php/thehub/thehub-settings.inc ~/sources/aws-codecommit/cob-drupal-hub/nobackup/

rsync -arz -essh -P thehub.prod@thehub.ssh.prod.acquia-sites.com:/var/www/site-scripts/site-info.php ~/sources/aws-codecommit/cob-drupal-hub/nobackup/

