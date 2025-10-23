---
title: Using Your Own AWS Account
weight: 22
---

::alert[Note, you might occur charges for provisioned resources below. The amount depends how long you will be experimenting with Coder and how many workspaces you will provision.]{type="warning"}

## Workshop resources

The following resources will be deployed to run the workshop:
- Amazon EKS cluster (`coder-aws-cluster`) with Auto Mode, which allows to automatically provision and manage compute capacity, networking, and storage based on your workload requirements. This reduces operational overhead while optimizing costs.
- VPC with public/private subnets across multiple Availability Zones
- EKS node groups with optimized instance types
- AWS Load Balancer Controller and EBS CSI driver
- IAM roles and policies for secure AWS service integration
- PostgreSQL database for Coder persistence
- Coder control plane services and web interface
- Network Load Balancer for external access
- CloudFront distribution for secure content delivery
- Kubernetes Service accounts and additional IAM roles for workspace provisioning

## Key Benefits of This Architecture

- **High Availability**: Multi-AZ deployment ensures resilience
- **Auto Scaling**: EKS Auto Mode automatically manages capacity
- **Security**: Encrypted storage, private networking, and least-privilege IAM
- **Cost Optimization**: Pay-per-use scaling and efficient resource utilization

::alert[The role used to bootstrap the account will require sufficient permissions to provision the resources above.]{type="info"}

## Download and execute the CFN installation script

Download Cloud Formation Template :link[**here**.]{href=":assetUrl{path="../static/infrastructure/eks-cluster.yaml"}" action=download} to automatically provision AWS instrustructure needed and to deploy Coder. Use AWS CLI or *AWS Management Console -> Cloud Formation -> Create Stack* and select the option *Upload a template file*. 

You will need to provider a stack name. You can leave the rest parameters as is.

::alert[Note the deployment takes 20-30 minutes. Time for a little break and coffee! Feel free to explore the content and Coder termonilogy and continue with hands-on when deployment completed!]{type="warning"}

::alert[If you are running this workshop on your own AWS account, remember to delete all resources by following the [Cleanup instructions](/5_conclusion/52_cleanup.html) to avoid unnecessary charges.]{type="warning"}
