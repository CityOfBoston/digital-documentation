# Non-Monorepo Service Setup

## Prerequisites

For web services that will run in containers, our system expects the following 2 behaviors:

* It listens for TLS on its `$PORT`
* It responds with a 200 to requests for `/admin/ok`

## Setting up the repository

Services are deployed into the Elastic Container Services \(ECS\) environments by cloning a tagged image held in the Elastic Container Register \(ECR\).

Our images are stored in the [AWS US-East-1 ECR](https://console.aws.amazon.com/ecr/repositories?region=us-east-1).  

You will first need to create a new **private** repository using the AWS console or the AWS CLI. The repository name should follow a convention: the environment \(stage/prod\) and service \(e.g. access-boston etc\) that is contained within the image. For example `cob-digital-apps-prod/access-boston`. 

{% hint style="info" %}
**Repo Creation Notes**

* The new repository name must start with a letter and can only contain lowercase letters, numbers, hyphens \(-\), underscores \(\_\), and forward slashes \(/\).
* AWS refer to `cob-digital-apps-prod/`part of the name as a _namespace_ "to group the repository into a category".  
* The digital monorepo deploy process presently requires one of 2 namespaces `cob-digital-apps-prod` and `cob-digital-apps-stage`. You do not need to follow this convention, but it is useful if your images need to be different for stage and prod.
{% endhint %}

**Using AWS CLI to create a new repo:**

```text
aws ecr create-repository \
     --repository-name my-repo-name \
     --region us-east-1
```



