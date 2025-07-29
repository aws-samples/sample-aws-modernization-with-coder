---
title: Using Your Own AWS Account
weight: 22
---

{{% notice warning %}}
If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/90-cleanup) to avoid unnecessary charges.
{{% /notice %}}

## Workshop resources

The following resources will be deployed to run the workshop:

- AWS CloudShell environment.
- AWS EKS Cluster with supporting VPC, Subnets, Elastic Load-Balancer, EC2 Node Group with EBS Storage and supporting IAM Roles.

{{% notice info %}}
The role used to bootstrap the account will require sufficient permissions to provision the resources above.
{{% /notice %}}

## Download and execute the bootstrap script

We will download the workshop Github repo, and the bootstrap script to configure additional utilities needed in the AWS Clooudshell. We will then run the bootstrap script to provision the utilities.

1. Sign in to the AWS Management Console and open the AWS CloudShell console at https://console.aws.amazon.com/cloudshell/.

2. In the menu bar, for **Select a Region**, choose the region in which you will be running the workshop.

3. Run the following commands in CloudShell terminal.
```bash
# Git Clone the Workshop Repo
git clone <workshop repo name>
```
```bash
# Change directories to the workshop repo
cd <workshop repo>
```
```bash
# Execture the bootstrap script
./workshop_setup.sh
```

{{% notice warning %}}
If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/90-cleanup) to avoid unnecessary charges.
{{% /notice %}}
