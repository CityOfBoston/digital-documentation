---
description: >-
  This page contains information for developers wishing to update or maintain
  the SQL Proxy endpoint on the dbconnector service.
---

# Developer Notes

## Developer Setup

The code for the dbconnector is stored in the AWS codecommit [cob\_dbconnector](https://us-east-1.console.aws.amazon.com/codesuite/codecommit/repositories/cob\_dbconnector/browse?region=us-east-1) repository (CoB Digital account).

Typically the code is cloned from that repository onto the developers local machine. In order do this, AWS credentials must set up correctly.  See these notes.

```
cd ~/sources/
git clone ssh://git-codecommit.us-east-1.amazonaws.com/v1/repos/cob_dbconnector
```

Once the repo is cloned, and assuming cloned into `~/sources/cob_dbconnector,`then the following commands will build the app, build its container locally and tag the container:&#x20;

```
cd ~/sources/cob_dbconnector
./scripts/build.sh local
```

The script will create a docker container, build the necessary files from the local cloned folders and then save and tag the docker image.

The developer can then us docker-compose to start the container as follows:

```
cd ~/sources/cob_dbconnector
docker-compose up --no-build -d dbconnector
```

This will build the container locally, start it and name it dbconnector.

## Modifying code

Once the steps in "Developer Setup" above have been completed, the developer can go ahead and change the code as needed in the cloned repository.

Committing code and p

## Testing

## Deploying
