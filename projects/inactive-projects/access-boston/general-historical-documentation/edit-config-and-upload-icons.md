---
description: Proposed Ways to Hand-off Config Editing and Icon Upload to the Security team.
---

# Edit Config and Upload Icons

Brainstorm/Plan ways the security team can take owner ship of the deployment of new icons and updating Access-Boston’s config file.\
\
**2 Step Deploy Plan**: The Security will update the config files in S3 via SFTP and the Digital team will restart the Applications.

1. The first is giving them access to the S3 bucket directory containing the App config \`.yml\` file via SFTP. Once they make the changes they will create create an ‘Issue’ ticket in our Github project for the Digital team to schedule a deploy to the service.
2. The second part is triggering a deployment of their applications on AWS's [ECS](https://console.aws.amazon.com/ecs/home?region=us-east-1#/clusters). Depending on the details of the change, we would go corresponding instance in either "AppsProdDeploy" or “AppStagingDeploy” and 'force a new deploy’ by using the ‘Update” the instance button.

\
**Dedicated Git Repository Deploy Plan**: The Security team commits a “Pull Request” to a private repository, whose build process will trigger a build/deploy request through Travis-CI and restart the corresponding App containers.

1. Setup a private repository the Security team can update.
2. Replicate part of the deploy process (travis, slack, etc) the mono-repo uses update the Apps in AWS

\
**SFTP/S3 and LAMBDA Deploy**

1. Give the Security team's SFTP Access to the S3 bucket path containing the App config files.
2. Create a Lambda function that gets triggered when the contents of the S3 bucket are updated
3.
   1. Connect the corresponding Access-Boston environment using ECS CLI and ‘force new deploy’ that application.

Resources:

* [ECS Task Runner with Lambda](https://medium.com/@pahud/ecs-task-runner-with-lambda-4594b72ccb)
* [Better Together: Amazon ECS and AWS Lambda](https://aws.amazon.com/blogs/compute/better-together-amazon-ecs-and-aws-lambda/)
