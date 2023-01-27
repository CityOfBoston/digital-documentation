# AWS for Developers

## **Obtain AWS Credentials from Administrator**

Contact the AWS administrator to get credentials for logging into the AWS console and (if necessary) interacting with AWS via the command line.

## Add your SSH credentials to AWS

Once you have a login to the AWS console: if you wish to use the AWS-CLI, or use any other command line program which connects to AWS (e.g. git for CodeCommit) you will need to register/add an SSH key on your AWS-CLI account.

{% hint style="success" %}
You can use an existing ssh key, or create a new one.
{% endhint %}

## **AWS CLI**

You need to install the AWS CLI if you, or a tool you use, needa to interact with AWS from the command line - for example:

* To use terraform to maintain AWS
* To deploy webapps to AWS
* To modify AWS objects from the command line

### Install AWS-CLI

Follow the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

You want to install the AWS-CLI on you local machine, not inside a container.  Follow the Mac, Windows or Linux instructions accroding to the OS you are using.

### Add AWS-CLI credentials

Verify AWS is installed using LINUX console:

```
/usr/local/bin/aws --version
```

You should see an output something like:

```
aws-cli/2.7.4 Python/3.9.11 Linux/5.10.102.1 .....
```

If not then return to the "Install AWS-CLI" section above.

Obtain your secret access keys for AWS from the AWS administrator, and then create the AWS credentials file using the LINUX console:

```
echo "[cityofboston]" > ~/.aws/credentials
echo "aws_access_key_id = xxxxxx" >> ~/.aws/credentials
echo "aws_secret_access_key = xxxxx" >> ~/.aws/credentials
```

{% hint style="info" %}
Alternatively, you could create and edit the `~/.aws/credentials` file using any text editor.
{% endhint %}
