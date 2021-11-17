---
description: >-
  To access the AWS resources (e.g. EC2 devices) you first need to SSH into the
  AWS environment.
---

# AWS Bastion Access

You can access the SSH Bastion from the City Hall network \(`140.241.0.0/16`\) if you have an SSH key on your AWS account and are in the `SshAccess` IAM group.

1. Get an admin to add you to the `SshAccess` IAM group.
2. From the IAM console, upload a public key for your account
3. Edit your `/etc/hosts` to add the following line: `35.169.164.239 apps-bastion`
4. Initialize your account on the bastion by SSHing without a public key: `ssh -o PubkeyAuthentication=no <username>@apps-bastion` _Note: your bastion username is the bit before `@boston.gov` on your account name._
5. Control-C out when it asks for a password.
6. SSH in with your public key: `ssh -A <username>@apps-bastion` \(the -A forwards the SSH agent, which is important for SSHing onto the instances.\)

From the Bastion, you can get to cluster machines with `ssh ec2-user@<ip-address>`. Look up the IP addresses in the EC2 console. You will need to have the ec2-user SSH private keys loaded into your SSH agent. The .pem files containing the private keys and the passwords for them can be shared to you via Dashlane.

Once you’re on a container instance, you can use `docker` commands to inspect containers, and `docker exec -it <container name> /bin/sh` if you need shell access inside of one. Outside of the containers, the `ec2-user` account can use `sudo -s` to open up a shell with root access.

_**This is taken from the**_ [_**digital repository readme.md**_](https://github.com/CityOfBoston/digital-terraform#bastion-access)_\*\*\*\*_

