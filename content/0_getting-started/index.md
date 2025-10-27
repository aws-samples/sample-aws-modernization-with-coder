---
title: Getting Started
weight: 20
---

## Workshop Architecture

The following architecture diagram illustrates the comprehensive AWS and Coder integration we'll be deploying: 

![architecture diagram](/static/images/AWSCoderSingleRegionv1-1.png)

This architecture demonstrates a production-ready, scalable development platform that combines:

- **Amazon Elastic Kubernetes Service (EKS)** with Auto Mode for intelligent container orchestration
- **AWS Network Load Balancer** content delivery and Coder Workspace connectivity
- **Amazon Aurora PostgreSQL** for reliable data persistence
- **AWS Identity and Access Management (IAM)** for enterprise-grade security
- **Coder Control Plane** for cloud development environment management
- **AI Integration** with Amazon Q Developer and Anthropic Claude Code + Amazon Bedrock

## Preparing for the Workshop

The workshop infrastructure will be automatically deployed using AWS CloudFormation at AWS Event or can be manually provisioned via the AWS CLI. This includes the EKS cluster, networking, security components, and all foundational services needed for the Coder platform.

**Choose your setup path:**

- **AWS Guided Event**: If you're attending an AWS-hosted workshop, follow the setup instructions [here](/0_getting-started/01-aws-event.html).
- **Self-Paced Learning**: If you're running this workshop independently, follow the setup instructions [here](/0_getting-started/02-own-account.html).

## What Gets Deployed during the Workshop

✅ **Amazon EKS Cluster** with Auto Mode enabled  
✅ **Amazon VPC and Networking** with public/private subnets across multiple AZs  
✅ **Security Groups and IAM Roles** with least-privilege access  
✅ **Amazon EBS CSI Driver** for persistent storage  
✅ **AWS Load Balancer Controller** for ingress management  
✅ **Amazon Aurora PostgreSQL Database** for Coder persistence  
✅ **Amazon CloudFront Distribution** for global access  
✅ **Coder Control Plane** services and web interface  

You will configure:

🔧 **Development Templates** for different personas  
🔧 **AI Integration** with Amazon Q Developer and Claude  


::alert[**Estimated Deployment Time**: The CloudFormation stack takes approximately 15-20 minutes to deploy all infrastructure components. You can monitor progress in the AWS CloudFormation console.]{type="info"}

::alert[**Cost Management**: If you are running this workshop on your own AWS account, remember to delete all resources by following the [Clean Up Resources](/5_conclusion/52_cleanup.html) section to avoid unnecessary charges. The workshop is designed to use cost-effective instance types and auto-scaling to minimize costs during the learning experience.]{type="warning"}

### Ready to Begin?
Select your setup path above to start deploying your AI-powered development platform!
