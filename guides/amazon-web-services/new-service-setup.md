# New service setup

## Web service prereqs

For web services, our system expects the following 2 behaviors:

* It listens for TLS on its `$PORT`
* It responds with a 200 to requests for `/admin/ok`

## Repo files

In the repo, copy-and-paste these files from a recent, similar service.

* deploy/setup.yml
* deploy/deploy.sh
* .dockerignore
* .travis.yml
* Dockerfile
* entrypoint.sh

Mark entrypoint.sh and deploy/deploy.sh executable.

Edit deploy/setup.yml to change its description and the description in its dashboard Markdown to reflect the new service. Also change the default short name.

Update .travis.yml for Slack by running: `travis encrypt "cityofboston-doit:EUIp*************" --add notifications.slack.rooms` \(get the rest of the API key from the Slack UI\)

Check the `deploy` section of .travis.yml to make sure that the branches and destinations are suitable for the current stage of development.

Run `docker build .` to test that the container can build.

{% hint style="info" %}
It’s a good idea to edit .travis.yml to add `deploy` to the list of branches that Travis will run on, and switch the deployment branch to `deploy` as well. Name your branch “deploy”. This will let you more easily iterate on deployment problems rather than PRing to the `production` branch all the time.
{% endhint %}

## AWS

### **Shared load balancer**

Look at the shared resource list and add a new priority entry for your service in the ALB you’re going to use \(or claim an “unused” number\).

\(See below for picking the stack name and ALB you’ll put in here.\)

{% page-ref page="shared-resource-list.md" %}

### **Deploy user**

Under IAM &gt; Users click “Add user” to make a user for Travis deploys. \(Don’t do this if you’re adding production to a service that’s already deployed to staging; they’ll share a user.\) Give it the name travis-ci.org+. Enable “Programmatic access” and don’t give it any permissions.

**Note down the Access ID and Secret Access Key.** You’ll need them for configuring the Travis build environment.

### **CloudFormation**

In one window, go to CloudFormation and click "Create Stack" and load up the deploy/setup.yml file you just created. Here’s how to fill out the values:

_Worker services will not have all of these parameters._

**Stack name:** Apps- \(_e.g._ AppsStaging-ContactForm\) If you will have more than one service in the environment, use another dash to differentiate. \(_e.g._ AppsStaging-311Indexer-Devint2\)

**CloudFormationTemplatesBucket:** Copy this from the parameters of another stack.

**ClusterStack:** Should be AppsStaging or AppsProd

**ContainerPort:** What port your service listens on, either by default or through your `$PORT`configuration in .env. Usually 3000 for webapps.

**DeployUserName:** Either the user you created just above, or the current staging Travis user, if you’re adding a production build.

**Environment:** Either “staging” or “production”. Currently passed to the common / webapp resources templates but not used by anyone.

**LoadBalancerListenerPriority:** The claimed priority from the shared resource list document.

**LoadBalancerStack:** If you’re on staging, “AppsStagingAlb”. If you’re in production but don’t need Incapsula in front of your service, “AppsPublicAlb”. If you want an ALB that only allows City / Incapsula traffic, “AppsProdAlb”.

**PublicDnsName:** Fully-qualified domain name your app will be accessible at. Likely something like `short-name.digital-staging.boston.gov`

**ServiceDesiredCount:** Number of instances you intend to run. **Used only for monitoring.** The deploy script is where the actual number of running services is set. \(It’s up to you to keep these consistent.\)

**ShortName:** kebab-case name for your service. Used in making log streams, ECS registries, _&c._Should be close to the GitHub repository name, and the kebab-case translation of the ShortName from the “Stack name” field. Shouldn’t be too long.

Click **Next**. Under **Advanced** set the “Notification options” to the “Existing Amazon SNS topic” of “AppsSns-Deploy”. This will send deploy / rollback notices to Slack via the Shippy-Toe service.

Click **Next** again, acknowledge that IAM resources will be created, and click **Create**.

After a few minutes \(many minutes if a CloudFront distribution is set up\) you should have a CloudFormation stack for your service and either one or two substacks of common resources. Now on to Travis to set up your first deploy.

### Travis

It is Travis’s job to run tests and, for changes on “staging” and “production” branches, build a Docker image, push it to the repository, and create a CloudFormation changeset that, when applied, will deploy the new code.

Before going live, a service’s production branch should deploy to a staging environment. After go-live, it should go to production instead, with a staging branch available for force-pushing code to test internally. \(Most changes should go directly to prod.\)

Copying a .travis.yml file from a working repository should mean that the following things are set up:

* A “trusty” distribution with the “docker” service enabled
* Running on all PRs but only staging, production, and [Greenkeeper](https://greenkeeper.io/) branches
* Installing the `awscli` command-line tool in a `before_deploy` step
* Running `deploy/deploy.sh` with `$STAGING_` environment variables on the production branch.

### **Environment Variables**

Now, all you need to do next set up is a million environment variables. We make as many public as we can to make it easier to debug and track down things.

Set these environment variables under the “Settings” section of the Travis configuration. Toggle on the “Display value in build log” for all “public” variables. \(It defaults to “Off”\)

**AWS\_ACCESS\_KEY\_ID** _public_ — The access key for the travis-ci+XXXXX deploy user you created above via the IAM UI. If you’ve lost the authentication info, reset it in the IAM interface.

**AWS\_DEFAULT\_REGION** _public_ — us-east-1

**AWS\_SECRET\_ACCESS\_KEY** — Secret key from the Travis deploy user.

**DEPLOY\_NOTIFICATION\_ARN** — The ARN \(starting with `arn:aws:…`\) of the AppsSns-Deploy topic. Look this up in the Simple Notification Service &gt; Topics UI. We keep it private because it contains our AWS account ID.

**DEPLOY\_TEMPLATE\_S3\_HTTPS** _public_ — HTTPS URL starting with `https://cob-digital-cf-templates.s3.amazonaws.com/service/` Likely either the `default-webapp-service.yml` or `default-worker-service.yml`, though it could also be a service-specific template under the `custom` subdirectory. \(This variable used to be public to obscure the bucket name, but it’s all access-controlled anyway.\)

**STAGING\_APP\_STACK** _public_ — Name of the stack created above for the service. _E.g._“AppsStaging-RegistryCerts”

**STAGING\_CLUSTER\_STACK** _public_ — “AppsStaging” or “AppsProd”

**STAGING\_DESIRED\_COUNT** _public_ — Number of instances to actually run. You should start this at 0 to get the stack created without worrying about configuration or passing health checks with running service tasks.

**STAGING\_REPOSITORY\_NAME** — Look this up under Elastic Container Service &gt; Repositories. The UI refers to it as the “Repository URI”. Kept private because it has our account ID.

### **Creating the Deploy Stack**

The Travis deploy user does not have permission to create a CloudFormation stack, just to propose a changeset for it. So, you’ll need to create the stack yourself before the Travis deploy can succeed.

To do this, you’ll need many of the values from above.

In the CloudFormation UI, create a new Stack.

1. For the template, paste in the `DEPLOY_TEMPLATE_S3_HTTPS` URL
2. The “Stack name” should be your service’s stack name plus “-Deploy” on the end
3. For the Parameters, fill in matching values from above. Leave GitBranch and GitRevision blank. The Tag should default to “latest”. Keep that default.
4. On the next page, under Advanced, set the SNS topic to AppsSns-Deploy, as before.
5. Create the stack.

### **Test Deploy from Travis**

Now go back and push your branch so that Travis sees it and runs a build. It should create a changeset, and Shippy-Toe should message the \#digital\_builds channel with the command line to execute it.

Make sure that you’re logged in to AWS command line tools with an account in the DIGITAL-ADMIN-LOCAL-CLI group and paste that command into a terminal window.

Monitor the deployment in the CloudFormation UI.

### DNS

You’ll need to get public DNS to point your new domain name to the ALB. Under EC2 &gt; Load Balancing &gt; Load Balancers find your ALB and copy its “DNS name”.

Next, email Frank to ask for a CNAME from your service’s public DNS from above to this ALB address.

### Configuration

Likely, before your service can start working, you’ll need to add .env and TLS configuration for it. This is done through an S3 bucket, either “digital-apps-staging-config” or “digital-apps-prod-config”. The default Docker entrypoint.sh for our services automatically downloads the contents of the service’s sub-path in the bucket to the app’s directory before starting it.

You’ll need to create a directory in the appropriate bucket named with your service’s kebab-case short name and add files to it. For most services that will be a .env file, and for web services a server.key and server.crt file as well.

The directories in this bucket are protected so that each service can only read its own sub-path.

For information on how to KMS-encrypt secrets, see encrypting service configuration.

{% page-ref page="encrypting-service-configuration.md" %}

### **TLS Setup**

The ALB -&gt; service communication must be over TLS, but it’s ok to use a self-signed certificate. Here’s the command line for creating a fresh server.key and server.crt, though you can also just copy from another service.

```text
$ openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
$ openssl rsa -passin pass:x -in server.pass.key -out server.key
$ openssl req -new -key server.key -out server.csr
$ openssl x509 -req -sha256 -in server.csr -signkey server.key -out server.crt
```

### Running the service

You can test that your service starts up and can serve traffic by going to Elastic Container Service &gt; Clusters. Pick your cluster and “Update” your service’s “Number of tasks”.

You should see things update and reach a “Steady state”. If you go to EC2 &gt; Load Balancing &gt; Target Groups, you should see a healthy target in the service’s target group.

Monitor whether there are many task creations in the service’s events, which is a sign of flapping. If there is, or the service does not register as healthy, check the logs in CloudWatch.

## Tips for Rails

* If running with Puma, get SSL listening by having a `CMD` of: `CMD ["bundle", "exec", "puma", "-b", "ssl://0.0.0.0:3000?key=server.key&cert=server.crt"]`
* Set the `RAILS_LOG_TO_STDOUT` env variable to `1` in .env
* Ensure that the `dotenv-rails` Gem is included in all environments, not just development / test

