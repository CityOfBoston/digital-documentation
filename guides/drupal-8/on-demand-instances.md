---
description: >-
  City of Boston use Terraform to create on-demand instances on AWS, or else
  attach a GitHub branch to a spare Aquia environment.
---

# On Demand Instances

On demand instances of the Drupal site \(boston.gov or hub\) are useful to demonstrate new features or functionality sand-boxed away from the Continuous-Deployment process. 

{% tabs %}
{% tab title="Acquia" %}

{% endtab %}

{% tab title="AWS" %}
You can push your local repository up to a test instance on our staging cluster on AWS. This will let you show off functionality using data from a staging snapshot of Boston.gov.

### **Prerequisites**

* You will need a full development environment and Drupal 8 installed on your local machine \(refer to earlier notes\).
* Install the [AWS Command Line Interface](https://aws.amazon.com/cli/).
* Get a “CLI” IAM user with an access key and secret key.
* Use `aws configure` to log your CLI user in locally. Use `us-east-1` as the

  default region.

{% hint style="info" %}
Request your CLI IAM user credentials from DoIT.
{% endhint %}

### **Setup On Demand \(Infrastructure\)**

To create a place to upload your code, follow the instructions in the [CityOfBoston/digital-terraform](https://github.com/CityOfBoston/digital-terraform) repository to make a “variant” of the Boston.gov staging deployment.

### **Pushing local code**

To push your local repository up to the cluster, run:

```text
$ ./doit stage <variant>
```

Where `<variant>` is the variant name you created in `CityOfBoston/digital-terraform`.

This will build a container image locally and upload it to ECR. It will then update your staging ECS service to use the new code.

By default, the container startup process will initialize its MySQL database with a snapshot of the staging environment from Acquia.

After the container starts up and is healthy, the `doit` script will print useful URLs and then quit.

### **Running drush on staging**

Direct SSH access is not generally available on the ECS cluster. To run `drush` commands on your test instance, you can visit the `webconsole.php` page at its domain. This will give you a shell prompt where you can run _e.g._ `drush uli` to get a login link.

The `webconsole.php` shell starts in `docroot`.

{% hint style="info" %}
Talk to another DoIT developer to get the webconsole username and password.
{% endhint %}

### **Preserving the database between pushes**

**NOTE**: Each time you deploy code to your test instance it starts with a fresh copy of the Drupal database.

If you want to preserve state between test runs, log in to `webconsole.php` and run:

```text
$  ../doit stash-db
```

\(The `..` is because `webconsole.php` starts in the `docroot`.\)

This will take a snapshot of your database and upload it to S3. The next time your test instance starts up, it will start its sync from this database rather than the Acquia staging one.

{% hint style="danger" %}
The database will also be destroyed when the AWS containers are restarted for any reason.  It is good practice to stash your DB regularly.
{% endhint %}

To clear the stash, so that your database starts fresh on the next test instance push, use `webconsole.php` to run:

```text
$ ../doit stash-db reset
```
{% endtab %}
{% endtabs %}

