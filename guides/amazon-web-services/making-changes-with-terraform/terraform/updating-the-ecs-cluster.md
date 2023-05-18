---
description: How to update the AMI on our ECS cluster instances
---

# Updating the ECS cluster AMI

The Digital webapps cluster uses the Elastic Container Service on AWS. We have a handful of EC2 instances that actually host the containers.

These instances use a stock Amazon Machine Image (AMI) from Amazon designed for Docker that comes with the ECS agent pre-installed. From time to time, Amazon releases a new version of this “ECS-optimized” image, either to upgrade the ECS agent or the underlying OS.

Thanks to our [instance-drain Lambda function](https://github.com/CityOfBoston/digital-lambda/tree/production/instance-drain), updating the cluster EC2 images is a zero-downtime process. Nevertheless, it’s best to run this during the weekly digital maintenance window, and make sure that staging looks good before doing it on production.

This process is sometimes referred to as “rolling” the cluster though it’s more accurate that we set up a second cluster of machines and migrate to it.

## Performing the AMI update from GitHub

1. Find the latest ID for the ECS-optimized AMI. You can do this on the [Amazon ECS-optimized AMIs page](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image\_id/description?region=us-east-1).&#x20;
2. In a browser navigate to [CityOfBoston/digital-terraform](https://github.com/CityOfBoston/digital-terraform/) repository and edit the [**`apps/clusters.tf`**](https://github.com/CityOfBoston/digital-terraform/blob/production/apps/clusters.tf) file.&#x20;
3. Update the `instance_image_id` value for the `staging_cluster` module to the new AMI ID from step 1 above. Save/commit the file as a new branch, not directly to the `production` branch.
4. Make a PR which merges the new branch into the `production` branch, and assign a person to review the changes.
5. When you make the PR, GitHub will automatically execute an `atlantis plan` process ([see what atlantis is](updating-the-ecs-cluster.md#what-happens-during-a-terraform-atlantis-apply)). \
   When the plan is done, inspect the output and expect to see changes to:\
   &#x20;\- resource "aws\_autoscaling\_group" "instances"\
   &#x20;\- resource "aws\_cloudwatch\_metric\_alarm" "low\_instance\_alarm"\
   &#x20;\- resource "aws\_launch\_configuration" "instances" _(This last one will have the new AMI guid)_\
   **Any other changes the plan identifies should be carefully investigated.** \
   **Terraform may be proposing to make changes to the AWS environment you don't want, or at least are not expecting.**&#x20;
6. After viewing the plan, if you need to update the terraform scripts, be sure to save the changes to the new branch.  \
   \
   <mark style="color:purple;">**If comitting your changes does not trigger the atlantis plan automatically, you can run it manually by creating a new comment with**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**`atlantis plan`**</mark><mark style="color:purple;">**.**</mark>\

7. Once the atlantis plan is finished, and the PR has been approved, create a new comment `atlantis apply.` This will cause Atlantis to apply changes to AWS. (Atlantis runs a `terraform apply` command in a background process). [See what happens](updating-the-ecs-cluster.md#what-happens-during-a-terraform-atlantis-apply).&#x20;
8.  Keep an eye on the “ECS Instances” tab in the cluster’s UI. You should see the “Running tasks” on the draining instance(s) go down, and go up on the new instances. \


    <figure><img src="../../../../.gitbook/assets/image.png" alt=""><figcaption><p>ECS Instances tab in the AWS web console</p></figcaption></figure>
9. Once all the tasks have moved, the old instance(s) will terminate and Terraform will complete. Check a few URLs on staging to make sure that everything’s up-and-running.
10. Now that Atlantis’s apply finished, you can merge the staging PR and repeat the process (steps 2-6) for the production cluster.

## Performing the AMI update using Terraform CLI

If you have [terraform installed](./#installing-terraform) on your local computer, you can do the update directly from your computer.

1. Find the latest ID for the ECS-optimized AMI. You can do this on the [Amazon ECS-optimized AMIs page](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image\_id/description?region=us-east-1).&#x20;
2. Ensure your cloned copy of the `digital-terraform` repository is on the `production` branch, and that the branch it up to date with the origin on GitHub.
3. Create a new branch from the `production` branch.
4. In your preferred IDE open the `/apps/clusters.tf` file and update the `instance_image_id` value for the `staging_cluster` module to the new AMI ID from step 1 above. Save/commit the file to the new new branch (not directly to the `production` branch).
5. in a terminal/shell from the `repo/apps/`folder, run the command:\
   `terraform plan`
6. When the plan is done, inspect the output and expect to see changes to:\
   &#x20;\- resource "aws\_autoscaling\_group" "instances"\
   &#x20;\- resource "aws\_cloudwatch\_metric\_alarm" "low\_instance\_alarm"\
   &#x20;\- resource "aws\_launch\_configuration" "instances" _(This last one will have the new AMI guid)_\
   **Any other changes the plan identifies should be carefully investigated.** \
   **Terraform may be proposing to make changes to the AWS environment you don't want, or at least are not expecting.**&#x20;
7. Once you are happy with the changes that terraform will apply to the AWS environment, you can run the command:\
   `terraform apply`\
   [See what terraform apply does](updating-the-ecs-cluster.md#what-happens-during-a-terraform-atlantis-apply).&#x20;
8.  Keep an eye on the “ECS Instances” tab in the cluster’s UI. You should see the “Running tasks” on the draining instance(s) go down, and go up on the new instances. \


    <figure><img src="../../../../.gitbook/assets/image.png" alt=""><figcaption><p>ECS Instances tab in the AWS web console</p></figcaption></figure>
9. Once all the tasks have moved, the old instance(s) will terminate and Terraform will complete. Check a few URLs on staging to make sure that everything’s up-and-running.
10. Now that terraform's apply is finished, you can repeat the process (steps 2-9) for the production cluster.
11. Finally you should merge the changes in your new (local) branch into the local `production` branch, and then push the your local `production` branch to the origin in Github.

{% hint style="success" %}
After the production instances are fully up, check that they have roughly equal “Running tasks” numbers. ECS should schedule duplicate tasks on separate machines so that they are split across AZs. If you see a service has both of its tasks on the same instance you can run a force deployment to restart it. (See [Restarting an ECS service](../restarting-an-ecs-service.md))
{% endhint %}

<details>

<summary>What are Atlantis and Terraform ?</summary>

<mark style="color:blue;">**Terraform**</mark> is a CLI utility synchronizes AWS with scripts. In essence, it uses a series of scripts to detect and make changes to AWS.  Terraform commands are run from a terminal session on a machine with Terraform libraries installed.\
[Installing Terraform](./#installing-terraform) | [See website](https://developer.hashicorp.com/terraform/intro) | [See documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

<mark style="color:blue;">**Atlantis**</mark> provides a GitHub provisioned wrapper for Terraform: it runs `terraform plan` and `terraform apply` commands from GitHub and posts the results back to GitHub.\
[See website](https://www.runatlantis.io/guide/) | [See documentation](https://www.runatlantis.io/docs/)\
Atlantis _is a small application which Digital team have installed on a very small serverless environment in AWS (fargate). It runs in fargate because it restarts the staging and production containers and therefore cannot run on any of the main EC2 instances._&#x20;

</details>

<details>

<summary>What happens during a terraform/atlantis apply with an updated AMI?</summary>

When the AMI is updated the `terraform plan` command will:

* create a new Launch Configuration for cluster instances (i.e. EC2 instances) that uses the new AMI,&#x20;
* create a new Autoscaling Group that uses the Launch Configuration,
* trigger deletion of the old Autoscaling Group.

The instance-drain _**Lambda**_ function will tell ECS to drain the tasks from the instances that are being shut down (terraform won’t delete the Autoscaling Group until its instances are fully terminated). ECS will automatically start those tasks up on the new instances that got created by the new Autoscaling Group.

</details>

