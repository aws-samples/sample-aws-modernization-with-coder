---
title: Using Your Own AWS Account
weight: 12
---

:::alert{header="Important" type="warning"}
If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/90-cleanup) to avoid unnecessary charges.
:::

## Workshop resources

The following resources will be deployed to run the workshop:
<!-- CHANGE THIS TO LIST ALL THE RESOURCES AND INITIAL SETUP NEEDED FOR THE WORKSHOP -->

- AWS Cloudshell environment.
- AWS EKS Cluster with supporting VPC, Subnets, Elastic Load-Balancer, EC2 Node Group with EBS Storage and supporting IAM Roles.

:::alert{header=Note}
The role used to bootstrap the account will require sufficient permissions to provision the resources above.
:::

## Download and execute the bootstrap script

We will download the workshop Github repo, and the bootstrap script to configure additional utilities needed in the AWS Clooudshell. We will then run the bootstrap script to provision the utilities.

1. Sign in to the AWS Management Console and open the AWS CloudShell console at https://console.aws.amazon.com/cloudshell/.

2. In the menu bar, for **Select a Region**, choose the region in which you will be running the workshop.

3. Run the following commands in CloudShell terminal.

:::code{showCopyAction=true showLineNumbers=false language=bash}

# Git Clone the Workshop Repo

git clone <workshop repo name>

# Change directories to the workshop repo

cd <workshop repo>

# Execture the bootstrap script

./workshop_setup.sh
:::

:::alert{header="Important" type="warning"}
If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/90-cleanup) to avoid unnecessary charges.
:::
