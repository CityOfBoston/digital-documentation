---
description: >-
  This page contains information for developers wishing to update or maintain
  the SQL Proxy endpoint on the dbconnector service.
---

# Developer Notes

## Developer Setup

The code for the dbconnector is stored in the AWS codecommit [cob\_dbconnector](https://us-east-1.console.aws.amazon.com/codesuite/codecommit/repositories/cob\_dbconnector/browse?region=us-east-1) repository (CoB Digital account).

Typically the code is cloned from that repository onto the developers local machine. In order do this, AWS credentials must set up correctly.  See [these notes](../../../../drupal-8/development-environment/aws-for-developers.md). The AWS-CLI is required, so it must be installed locally, and credentials must be set in the `~/.aws/credentials` file.

This command will clone the `develop` branch of`cob_dbconnector` application into a folder at `~/sources/cob_dbconnector`. The file may be cloned anywhere on the local computer, however this document assumes it will cloned into `~/sources/cob_dbconnector`and if it is not, then commands given here will need to be modified for the new location.

```
cd ~/sources/
git clone --branch develop ssh://git-codecommit.us-east-1.amazonaws.com/v1/repos/cob_dbconnector
```

Once the repo is cloned, then the following command will build the app, build its container locally and tag the container:&#x20;

```
cd ~/sources/cob_dbconnector
./build.sh local
```

The script will create a docker container, build the necessary files from the local cloned folders and then save and tag the docker image.

The developer can then use docker-compose to start the container as follows:

```
cd ~/sources/cob_dbconnector
docker-compose up --no-build -d dbconnector
```

This will build the docker container locally, start it and name it `dbconnector`.

## Modifying Code

Once the steps in "Developer Setup" above have been completed, the developer can go ahead and change the code as needed in the cloned repository.

In this example the code is found in `~/sources/cob_dbconnector/src`.

Best practice is to create a working branch off the `develop` branch and then make commit changes to the working branch as needed.  Once ready to push and merge code back to the repo in CodeCommit, the working branch can be pushed, and then a PR made and committed.  For example, to add a period to the end of the README.md file:

```
# Make sure the develop branch is up to date with the remote
git pull origin develop

# create a new local branch and check it out.
git checkout -b working_branch

# add a peiod at the end of the readme file
echo "." >>README.md

# add the readme file to the queue and commit the queue
git add README.md
git commit -mCommit to the working_branch

# push the changes to CodeCommit (and create new branch on remote)
git push -u origin develop

# delete the working branch locally 
#   The local branch is no longer needed, the PR will be done in CodeCommit
git branch -d working_branch
```

Now login to AWS and find the `cob_dbconnector` repo in CodeCommit. Create a PR from the `working_branch` into `develop`.  Use approval workflows as needed.

{% hint style="info" %}
Unlike the Drupal deploy process (where the deploy is triggered by commits to the GutHub repo) there are no triggers fired by committing, pushing, tagging or merging to the cob\_dbconnector CodeCommit repository. &#x20;

Deployment is triggered by updates to the ECR repository.
{% endhint %}

## Testing

Once changes have been made and saved in the `src` folder, the application needs to be re-built and the container re-deployed in order to verify the changes work as expected.

### Updating local container with updated/changed code

The repository files on the host are NOT mounted into the container.  Therefore changes to code on the host machine are not automatically replicated to the container, even if a local build is forced.  In order to "load" changed code into the container, it must be rebuilt.

To re-build and re-deploy the container:

```
cd ~/sources/dbconnector
./build.sh local 
docker stop dbconnector 
docker rm dbconnector 
docker-compose up --no-build -d dbconnector
```

Or more concisely (ensure you are in the correct folder):

```
./build.sh local && docker stop dbconnector && docker rm dbconnector && docker-compose up --no-build -d dbconnecto
```

### Manual testing in the local container

Because the dbconnector is an API designed to be used via an REST endpoint, the best tool for testing is either a custom written testbed which calls the container, or else a tool like postman.

To connect to the container for testing, you will need to know the containers IPAddress. This can be found as follows:

```
docker container inspect -f '{{ .NetworkSettings.Networks }}' dbconnector    
# select a network from the output and then ...
docker container inspect -f '{{ .NetworkSettings.Networks.xxx.IPAddress }}' dbconnector
```

The IPAddress will typically be something like `172.x.0.x`, depending on your docker setup. Locally the service runs on port 3000, and the conatiner has port 3000 exposed.

https://\<IPAddress>:3000/ should be used as the endpoint for testing locally: \
**For example** In postman run a test GET request on [the URL](../sql-proxy-2021.md#heartbeat) :

```
https://172.20.0.4:3000/admin/ok
```

This should return an empty json message `{}` with an HTTP Code of 200.  If you get anything else there is an issue.

#### Attach to console

Using docker you can attach to the running console for the dbconnector container and get a tail of the stdout and stderr in real-time.  This is useful because you can add `console.log()`commands to the code and then see them appear in the attached console.  \
Use this command to attach to the console:

```
docker attach dbconnector
```

#### Create a new interactive session (shell) on container

Using docker you can open a new bash shell in the container and run commands in the console for that session.  You could use this to verify the existence of files, or to check if dependencies are installed etc. This command creates an inetractive shell:

```
docker exec -it /bin/bash
```

If you dont need an interactive shell, you just want to run a single command, you can use this:

```
docker exec -it /bin/bash -c "ls -la /app/tmp"
```

### Automated Testing

There are a suite of automated tests created for the dbconnector.  These tests are run locally by the developer, and the tests can be extended.

The stock tests adequately test all the endpoints for the service, and as new functionality or features are added, the tests should be expanded to ensure they are up to date.

The tests are run using this command:

```
docker exec -it dbconnector /bin.bash -c"npm run test"
```

The tests are defined in the file `src/tests/tests.config.js`.

<details>

<summary>Test Configuration</summary>

The configuration defines an object with 2 top level elements, `config`, `tests.`

Typically, the `configs`are set properly and will not need changing. The only exception would be if a new class of user or user permission were being implemented and a new account needs to be provisioned to test this.

The `tests` section however will need updating when code and functionality are changed.

#### Test objects

The `tests` element contains 2 elements, `pdf` and `sql` each of which contains an array of test objects.  Each test object represents a test.

A test object is structured as follows:

```json
{
    description: [optional] string,
    enabled: bool,
    debug: bool,
    path: string,
    use_creds: [optional] int,
    save: [optional] string,
    method: {method_object},
    expected_response: {response_object}
}
    where:
    description = An explanation of what is being tested.
    enabled = Whether this test should be executed.
    debug = Should additional process information be output.
    path = enpoint (excluding scheme://domain which is defined in config element).
    use_creds = Use previously generated and saved credentials.
    save = If not empty, a name used to save response (see dynamic arguments).
    method = method_object = {
        type: string,
        payload: [optional] {},
        querystring: [optional] {}
    }.
        where:
        type = GET | POST | PATCH | DELETE the type of call to the path.
        payload = for type=POST|PATCH|DELETE, a JSON object with the payload.
        querystring = for type=GET, a JSON object with key:value querystring fragments.
    expected_response = {response-object} = {
        narrative: [optional] string,
        code: int,
        json_data: bool,
        exact: [optional] {json-data},
        attachment: [optional] bool,
        size: [optional] int
    }.
        where:
        narrative = [optional] An explanation of what the test proved.
        code = [required] The expected HTTP_Code from test.
        json_data = [required] Whether a json response is expected.
        exact = [optional] If json_data = true, a JSON object which represents 
                the (exact) expected JSON response from the endpoint.
        attachment = [optional] if true, an attachment is expected in response.
        size = [optional] (if attachment=true) size of attachment.

NOTES:  1. if <code> does not equal the received code from the endpoint, 
           then the test is marked as failed.
        2. if <json_data>=true then the test is marked as false if the
           received body/payload is not a json string (empty string is ok).
        3. if <json_data>=true and <exact> is provided, then the test 
           is marked as failed if the recieved json payload (as an object)
           does not match the <exact> value (as an object).
        4. if <attachment>=true, the test is marked as failed if no 
           attachment is detected in the returned payload body.
        5. if <attachment>=true and <size> is provided, the test fails 
           if char length of the body returned is not equal to <size>.
        6. in all cases, if the endpoint does not exist, or has a 500 error, 
           the test is marked as failed.
```

</details>

## Deploying

There are 2 remote environments an AWS, `stage` and `production`.

To deploy to either, its a simple matter of creating a stage or production build and then pushing the image to the ECR registry.  A watch process monitors the ECR repository and changes to the container images (uploads or re-tags) automatically prompts deploys to the ECR environment.

For the DBConnector there is a helper script to build, tag and push container images:

```
./scripts/build.sh stage

or

./scripts/build.sh prod
```

{% hint style="info" %}
This is all that is needed to initiate a deploy.
{% endhint %}

## Best Practice Developer Workflow

The might be the way a developer works:

### Project Start

* follow setup instructions [here](developer-notes.md#developer-setup)
* create a local working branch from `develop` branch&#x20;

### Project Development & Local Testing

* [alter code](developer-notes.md#modifying-code)
* commit code locally to working branch
* [build code locally](developer-notes.md#updating-local-container-with-updated-changed-code)
* [test locally](developer-notes.md#manual-testing-in-the-local-container)

... iterate these steps as necessary

### Project External Testing

* [update automated test rules](developer-notes.md#test-configuration)
* [run automated tests locally](developer-notes.md#automated-testing)
* commit code and push to AWS Code Commit
* create PR on CodeCommit (follow peer review and other related project steps) \
  and then merge PR (on CodeCommit)
* [build and deploy stage version](developer-notes.md#deploying) (stage)
* advise external testers

... iterate back through development stage steps as needed.

### Project End

* [build and depoy production](developer-notes.md#deploying) (production)
