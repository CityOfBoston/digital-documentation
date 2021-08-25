---
description: This covers deploying (and using) the deploy tool.
---

# Deploy Tool \(cob\_ecrDeploy\)

The application uses several AWS resources, including Lambda functions and an EventBridge Rule. These resources are defined in the `template.yaml` file in the project. You can update the template to add AWS resources as needed.

### Clone the application

To clone the cob\_ecrDeploy source code to your local machine you need the following tools.

* git
* AWS Credentials

#### AWS Credentials

To clone the repo you will need to have an [SSH Key registered](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html#setting-up-ssh-unixes-keys) in your AWS User account.

Then the repo can be cloned into any directory with this command:

```bash
git clone ssh://git-codecommit.us-east-1.amazonaws.com/v1/repos/cob_ecr_deploy
```

### Develop, build and test locally

{% hint style="success" %}
**The SAM CLI**

To Develop and Deploy this application, you will need the Serverless Application Model Command Line Interface \([SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)\). SAM CLI is an extension of the AWS CLI that adds functionality for building and testing Lambda applications

To use the SAM CLI, you need the following tools.

* AWS Access Keys - [Create credentials file](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-set-up-credentials.html)
* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* [Python 3 installed](https://www.python.org/downloads/)
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

You can also download and use the **AWS Toolkit** plug-in in your preferred integrated development environment \(IDE\).  The AWS Toolkit provides extensions to use the SAM CLI, and also adds step-through debugging experience for Lambda function code. 

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

#### Build locally

The code contained within the repo needs to be built before it can be run, deployed or tested.

Build the application with the `sam build` command from the root folder of the locally cloned cob-ecr_-_deploy repo.

```bash
cob_ecr_deploy$ sam build
```

The SAM CLI installs dependencies defined in `ecrDeploy_function/requirements.txt`, creates a deployment package, and saves it in the `.aws-sam/build` folder.

#### Running and debugging

Test a single function by invoking it directly with a test event. An event is a JSON document that represents the input that the function receives from the event source. Test events are included in the `events` folder in this project.

Run functions locally and invoke them with the `sam local invoke` command.

```bash
cob_ecr_deploy$ sam local invoke ecrDeploy_function --event events/event.json
```

With the AWS Toolkit, your IDE can be used to add breakpoints and step through code line by line.  See the box-out above for links to installation and debugging instructions for your IDE.

#### Add or edit a resource 

The application template uses AWS Serverless Application Model \(AWS SAM\) to define application resources. AWS SAM is an extension of AWS CloudFormation with a simpler syntax for configuring common serverless application resources such as functions, triggers, and APIs. For resources not included in [the SAM specification](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md), you can use standard [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) resource types.

The template is saved in `template.yaml` which is found in the application root folder.

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

### Deploy the cob\_ecrDeploy application

Deployment must be performed using the **SAM CLI**.  SAM CLI will use **Docker** to run the repo's functions in an Amazon Linux environment that matches Lambda.

The deployment settings are defined in the file `samconfig.toml`which is stored in the root of the repository.

To build and deploy your application, run the following in your shell, from the root of the repo:

```bash
cob_ecr_deploy$ sam build
cob_ecr_deploy$ sam deploy
```

### Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
aws cloudformation delete-stack --stack-name ecrDeploy-application
```

### Resources

See the [AWS SAM developer guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) for an introduction to SAM specification, the SAM CLI, and serverless application concepts.

Next, you can use AWS Serverless Application Repository to deploy ready to use Apps that go beyond hello world samples and learn how authors developed their applications: [AWS Serverless Application Repository main page](https://aws.amazon.com/serverless/serverlessrepo/)

