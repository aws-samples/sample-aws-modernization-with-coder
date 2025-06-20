---
title: Getting Started
weight: 3
---

## Workshop Architecture

The following architecture diagram illustrates the comprehensive AWS and Coder integration we'll be deploying:

![architecture diagram](/static/images/AWSCoderSingleRegionv1-0.png)

This architecture demonstrates a production-ready, scalable development platform that combines:

- **Amazon EKS** with Auto Mode for intelligent container orchestration
- **AWS Application Load Balancer** and **CloudFront** for global content delivery
- **Amazon RDS PostgreSQL** for reliable data persistence
- **AWS IAM** and **KMS** for enterprise-grade security
- **Coder Control Plane** for development environment management
- **AI Integration** with Amazon Q Developer and AWS Bedrock

## Preparing for the Workshop

The workshop infrastructure will be automatically deployed using AWS CloudFormation. This includes the EKS cluster, networking, security components, and all foundational services needed for the Coder platform.

**Choose your setup path:**

- **AWS Guided Event**: If you're attending an AWS-hosted workshop, follow the setup instructions [here](../0_getting-started/01-aws-event/).
- **Self-Paced Learning**: If you're running this workshop independently, follow the setup instructions [here](../0_getting-started/02-own-account/).

## What Gets Deployed Automatically

The CloudFormation template (`eks-cluster.yaml`) provisions:

✅ **Amazon EKS Cluster** (`coder-aws-cluster`) with Auto Mode enabled  
✅ **VPC and Networking** with public/private subnets across multiple AZs  
✅ **Security Groups and IAM Roles** with least-privilege access  
✅ **KMS Encryption Keys** for secrets and data protection  
✅ **EBS CSI Driver** for persistent storage  
✅ **AWS Load Balancer Controller** for ingress management  

## What You'll Configure During the Workshop

🔧 **PostgreSQL Database** for Coder persistence  
🔧 **Coder Control Plane** services and web interface  
🔧 **Development Templates** for different personas  
🔧 **AI Integration** with Amazon Q Developer and Claude  
🔧 **CloudFront Distribution** for global access  

{{% notice info %}}
**Estimated Deployment Time**: The CloudFormation stack takes approximately 15-20 minutes to deploy all infrastructure components. You can monitor progress in the AWS CloudFormation console.
{{% /notice %}}

{{% notice warning %}}
**Cost Management**: If you are running this workshop on your own AWS account, remember to delete all resources by following the [Clean Up Resources](/90-cleanup) section to avoid unnecessary charges. The workshop is designed to use cost-effective instance types and auto-scaling to minimize costs during the learning experience.
{{% /notice %}}

### Ready to Begin?
Select your setup path above to start deploying your AI-powered development platform!
