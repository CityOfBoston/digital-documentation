---
description: This covers deploying (and using) the deploy tool.
---

# Deploy Tool (cob\_ecrDeploy)

The application uses several AWS resources, including Lambda functions and an EventBridge Rule. These resources are defined in the `template.yaml` file in the project.&#x20;

### SAM CLI

{% hint style="success" %}
To develop and deploy this application, you will need the _Serverless Application Model Command Line Interface_ ([SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)).&#x20;

The SAM CLI is an extension of the AWS CLI that adds functionality for building and testing Lambda applications

To use the SAM CLI, you need the following tools.

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* [Python 3 installed](https://www.python.org/downloads/)
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition\&offering=community)

You will also need to:

* [Create AWS credentials file](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-set-up-credentials.html)

You can also download and use the **AWS Toolkit** plug-in in your preferred integrated development environment (IDE).  The AWS Toolkit provides extensions to use the SAM CLI, and also adds step-through debugging experience for Lambda function code.&#x20;

See the following links to get started with your preferred IDE:

* [PhpStorm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [PyCharm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [VS Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html)
* [IntelliJ](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [WebStorm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [CLion](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [GoLand](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [Rider](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [RubyMine](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [DataGrip](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/welcome.html)
{% endhint %}

### Cloning the application

To clone the [cob\_ecrDeploy source code ](https://console.aws.amazon.com/codesuite/codecommit/repositories/cob\_ecr\_deploy/browse/refs/heads/master?region=us-east-1)to your local machine you need the following tools.

* git
* AWS Credentials

#### AWS Credentials

To clone the repo you will need to have an [SSH Key registered](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html#setting-up-ssh-unixes-keys) in your AWS User account.

Then the repo can be cloned into any directory with this command:

```bash
git clone ssh://git-codecommit.us-east-1.amazonaws.com/v1/repos/cob_ecr_deploy
```

### Develop, build and test locally

#### Build locally

The code contained within the repo needs to be built before it can be run, deployed or tested.

Build the application with the `sam build` command from the root folder of the locally cloned cob-ecr_-_deploy repo.

```bash
cob_ecr_deploy$ sam build
```

The SAM CLI installs dependencies defined in `ecrDeploy_function/requirements.txt`, creates a deployment package, and saves it in the `.aws-sam/build` folder. &#x20;

#### Development

The actual work of deploying an ECR image is performed by the `lambda_handler()` function in the `app.py` file in the `ecrDeploy_function/ecrDeploy`folder.

* The `lambda_function()`is where you will likely need to make changes to the functions' interaction with ECS: i.e. stopping and starting tasks.
* The EventBridge rule which triggers the Lambda function is defined within the `template.yaml`file. \
  _You could also just modify EventBridge Rules in the AWS Console, but if you do that then any future deployment of this app may reset those changes._
* The IAM permissions for the Lambda function are defined in `template.yaml`and can be changed there.\
  Y_ou could also just modify the Lambda (or roles/policies in IAM) in the AWS Console, but if you do that then any future deployment of this app may reset those changes._
* Environment variables control the passing of variables into the application (tags/cluster-names etc).  These can be changed in the `template.yaml`file.\
  Y_ou could also just modify the Lambda in the AWS Console, but if you do that then any future deployment of this app may reset those changes._

{% hint style="info" %}
After making changes to `app.py`or `template.yaml`(or any other file in the project) save the files and test (see running and debugging sections). &#x20;

When satisfied, commit and push the changed code into the repository and then re-build and re-deploy the app using the SAM CLI (see next sections).
{% endhint %}

#### Running the application

Test a single function by invoking it directly with a test event. An event is a JSON document that represents the input that the function receives from the event source. Test events are included in the `events` folder in this project.

Run functions locally and invoke them with the `sam local invoke` command.

{% hint style="warning" %}
_Check the **event/event.json** file before running the command below - it will cause the image:tag specified to be deployed._
{% endhint %}

```bash
cob_ecr_deploy$ sam local invoke ecrDeployFunction --event events/event.json
```

{% hint style="info" %}
**Note:** This command will start a lambda-like docker container loaded with an AWS supplied Python image and the built app loaded into it. &#x20;

The app will be automatically run as if the even loaded in the `event.json` had fired, without any delay or any user input. &#x20;
{% endhint %}

#### Debugging the application

With the AWS Toolkit, your IDE can be used to add breakpoints and step through code line by line.  See the box-out above for links to installation and debugging instructions for your IDE.

#### Add or edit a resource&#x20;

The application template uses AWS Serverless Application Model (AWS SAM) to define application resources. AWS SAM is an extension of AWS CloudFormation with a simpler syntax for configuring common serverless application resources such as functions, triggers, and APIs. For resources not included in [the SAM specification](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md), you can use standard [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) resource types.

The application uses several AWS resources, including Lambda functions and an **EventBridge** Rule. These resources are defined in the `template.yaml` file in the project.&#x20;

#### Fetch, tail, and filter Lambda function logs

To simplify troubleshooting, SAM CLI has a command called `sam logs`. `sam logs` lets you fetch logs generated by your deployed Lambda function from the command line. In addition to printing the logs on the terminal, this command has several nifty features to help you quickly find the bug.

`NOTE`: This command works for all AWS Lambda functions; not just the ones you deploy using SAM.

```bash
cob_ecr_deploy$ sam logs -n ecrDeploy_function --stack-name cobEcrDeploy --tail
```

You can find more information and examples about filtering Lambda function logs in the [SAM CLI Documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-logging.html).

#### Unit tests

Tests are defined in the `tests` folder in this project. Use PIP to install the [pytest](https://docs.pytest.org/en/latest/) and run unit tests.

```bash
cob_ecr_deploy$ pip install pytest pytest-mock --user
cob_ecr_deploy$ python -m pytest tests/ -v
```

### Deploy the application

Deployment must be performed using the **SAM CLI**.  SAM CLI will use **Docker** to run the repo's functions in an Amazon Linux environment that matches Lambda.

The deployment settings are defined in the file `samconfig.toml`which is stored in the root of the repository.

To build and deploy your application, run the following in your shell, from the root of the repo:

```bash
cob_ecr_deploy$ sam build
cob_ecr_deploy$ sam deploy
```

### Cleanup

To delete the application after you have created it, use the AWS CLI or the [Cloud Formation pages](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringStatus=active\&filteringText=\&viewNested=true\&hideStacks=false\&stackId=) in the AWS Console.&#x20;

**AWS CLI -** Asuming the `samconfig.toml` file has not been used to change the `stack-name`, you can run the following:

```bash
$ aws cloudformation delete-stack --stack-name ecrDeploy-application
```

### Maintenance

From time to time, the python package versions and dependencies should be checked and updated as needed. &#x20;

To do this, you can specify actual package versions in `requirements.txt` and then run&#x20;

```
$ pip3 install -r requirememts.txt
```

You can update a specific package with this command:

```
pip3 install package==version 
```

You can check dependencies with this command:

```
pip3 check 
```

Your IDE may also be able to identify packages which should/could be updated. Or you can use this command

```
pip3 list --outdated
```

{% hint style="danger" %}
Take care with updating packages.&#x20;

Only packages which appear in the folder  `.aws-sam/build/ecrDeployFunction` need to be manually managed, there are a load of other packages pre-loaded within the AWS supplied container.

Really, running the `pip3 list --outdated` command should be run inside the container to get an accurate picture across all Python dependencies.
{% endhint %}

### Other Resources

See the [AWS SAM developer guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) for an introduction to SAM specification, the SAM CLI, and serverless application concepts.

Next, you can use AWS Serverless Application Repository to deploy ready to use Apps that go beyond hello world samples and learn how authors developed their applications: [AWS Serverless Application Repository main page](https://aws.amazon.com/serverless/serverlessrepo/)
