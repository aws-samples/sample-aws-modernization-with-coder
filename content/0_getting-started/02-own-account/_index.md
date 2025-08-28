---
title: Using Your Own AWS Account
weight: 22
---

{{% notice warning %}}
If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/5_conclusion/52_cleanup.html) to avoid unnecessary charges.
{{% /notice %}}

## Workshop resources

The following resources will be deployed to run the workshop:

- AWS CloudShell environment.
- AWS EKS Cluster with supporting VPC, Subnets, Elastic Load-Balancer, EC2 Node Group with EBS Storage and supporting IAM Roles.

{{% notice info %}}
The role used to bootstrap the account will require sufficient permissions to provision the resources above.
{{% /notice %}}

## Download and execute the helm installation script

We will be using the Kubernetes Helm utility from the AWS Clooudshell. If Helm is not already available in your Cloushell, we will then downlad the script to provision the utility.

1. Sign in to the AWS Management Console and open the AWS CloudShell console at https://console.aws.amazon.com/cloudshell/.

2. In the menu bar, for **Select a Region**, choose the region in which you will be running the workshop.

3. Run the following commands in CloudShell terminal.
```bash
# Install Helm using the official script
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get-helm-3.sh
chmod +x get-helm-3.sh && ./get-helm-3.sh
```
```bash
# Verify the installation
helm version --short
```

{{% notice info %}}
You may need to re-install **helm** if your AWS cloudshell session timesout and restarts.  Simply re-use the created `get-helm-3.sh` bash script.
{{% /notice %}}

{{% notice warning %}}
If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/5_conclusion/52_cleanup.html) to avoid unnecessary charges.
{{% /notice %}}
