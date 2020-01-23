---
description: >-
  Creates a drupal 8 container, mysql container and node container, and connects
  them all up.
---

# Installation instructions

## Quick install guide

For more detailed install and usage instructions for various platforms, see "**More Help**" below. 

Ensure you have set up your development environment as described here:

{% page-ref page="../development-environment/" %}

1. Clone the public [boston.gov-d8 repo](https://github.com/CityOfBoston/boston.gov-d8) into a local folder `git clone -b <branchname> git@github.com:CityOfBoston/boston.gov-d8.git` _\(City of Boston DoIT recommends that the **develop** branch be used\)_ 
2. On host computer, change directory to the repository root and use lando to create and start containers: `lando start`

{% page-ref page="typical-build-output.md" %}

{% hint style="info" %}
Depending on the power of the host machine, the Drupal 8 build process for boston.gov can take more than 15-20 minutes.  The composer install and site install \(esp. config import\) tasks can take 5-10 minutes each - with no updates being directed to the console.

-&gt; You can follow the process by inspecting the log files in `docroot/setup/`there are links to these files in the console.
{% endhint %}

{% hint style="success" %}
From the repository root \(on host\): 

* to view a list of available **lando** commands:`lando`
* to view **phing** tasks: `lando phing -l`
* to run **drush** commands:`lando drush <command>`
* `lando ssh`to login to docker container as `www-data`
* `lando ssh -user=root` to ssh and login as root
* `lando ssh <servicename>` servicename = appserver / database / node
{% endhint %}



## **Pro-tips**

### **Linux aliases**

To reduce typing at the console, you can add the following aliases to your `~/bashrc`, `~/.bash_aliases` or `~/.bash_profile` files on your development \(host\) OS.

```bash
alias ldrush='lando drush'
alias lssh='lando ssh'
alias lsshroot='lando ssh --user=root'
alias lphing='lando phing'
alias lcomposer='lando composer'
alias lnpm='lando npm'
alias ldrupal='lando drupal'
function lls() {
  lando ssh -c "ls -la $1"
}
```

With these aliases, typing \(in a console\) `lls <folder>` will use lando to run `ls -la <folder>` in the default container \(in our case appserver\) and list files there. Whereas, `ls <folder>` will list the folder locally \(i.e. on the host\) as usual.

## More help

For more information on installation, usage and administration of the development area, go to the next section.

{% page-ref page="../development-environment/" %}

