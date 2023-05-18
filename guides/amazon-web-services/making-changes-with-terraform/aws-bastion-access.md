---
description: >-
  To access the AWS resources (e.g. EC2 devices) you first need to SSH into the
  AWS environment.
---

# AWS Bastion Access

You can access the SSH Bastion from the City Hall network (`140.241.0.0/16`) if you have an SSH key on your AWS account and are in the `SshAccess` IAM group.

### Step 1: Set up access to the bastion

1. Request an AWS Admin to add you to the `SshAccess` IAM group.
2. From the IAM console, upload a public key for your account
3. Edit your `/etc/hosts` to add the following line: `35.169.164.239 apps-bastion`
4. Initialize your account on the bastion by SSHing without a public key: `ssh -o PubkeyAuthentication=no <username>@apps-bastion` _Note: your bastion username is the bit before `@boston.gov` on your account name._
5. Control-C out when it asks for a password.
6. SSH in with your public key: `ssh -A <username>@apps-bastion` (the -A forwards the SSH agent, which is important for SSH'ing on to the instances.)

### Step 2: Setup access to the EC2 instances

From the Bastion, you can get to the EC2 instances which host the ECS services.

1. Request that the AWS Admin share the ec2-user private keys and passwords with you via dashlane. There are 2 keys one for production and one for staging.  Save whichever you need, or both, into your `~/.ssh` folder.&#x20;
2. Ensure the permissions on the private key file/s are set to 600 (`chmod 600 xxxx`)
3. Note the `Private IPv4 address` of the EC2 instance from the [EC2 instances page](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:v=3;$case=tags:true%5C,client:false;$regex=tags:false%5C,client:false) in the AWS console - this will be `10.40.15.x` for staging and `10.40.115.x` for production. \
   _There are 2 production instances, you can use either._\
   _These IPAddresses change after each deployment, so check regularly._
4. Once you have successfully SSH'd onto the bastion (#6 in Step 1 above), you will be able to ssh onto the instance `ssh ec2-user@<ipaddress>`

### Step 3: Access a running container

Once you’re on a container instance (#4 step 2 above), you can use `docker` commands to inspect containers

for example some useful commands are:

```
docker stats     --> provides realtime container performance stats
docker ps        --> provides a list of currently provisioned and running containers

docker exec -it <containername> /bin/sh  --> shell access (like ssh) a container

docker stop <containername>           --> will stop a container, 
                                          auto scaling should cause it to restart
                                  
docker logs --follow <containername>  --> view and tail stdin and stderr
```

_<mark style="color:purple;">??? Outside of the containers, the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`ec2-user`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">account can use</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`sudo -s`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">to open up a shell with root access.</mark>_
