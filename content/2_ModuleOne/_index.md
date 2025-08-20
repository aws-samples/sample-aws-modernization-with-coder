---
title: "Coder Deployment"
chapter: true
weight: 30
---

# Coder Platform Deployment

Welcome to the Coder Platform Deployment section! In this module, you'll connect to your pre-deployed AWS infrastructure and configure the Coder Cloud Development Environment (CDE) control plane. 

The CloudFormation template has already provisioned the foundational AWS infrastructure. Now we'll deploy and configure the Coder components to create a production-ready cloud development environment platform.

## What We'll Accomplish

In this module, you will:

1. **Connect to Pre-Deployed Amazon EKS Cluster** - Access your provisioned EKS Kubernetes cluster with Auto Mode enabled
2. **Deploy PostgreSQL Database Service** - Set up persistent storage for Coder's configuration and workspace metadata
3. **Install Coder Control Plane** - Deploy the core Coder services with Network Load Balancer and ingress configuration
4. **Configure IAM Roles and Service Accounts** - Set up secure AWS integration for dynamic workspace provisioning
5. **Deploy AWS CloudFront Distribution** - Enable secure TLS termination for Coder workspace access

## Platform Deployment Overview

Here's what has been automatically deployed and what we'll configure:

**Pre-Deployed Infrastructure (via CloudFormation):**
- Amazon EKS cluster (`coder-aws-cluster`) with Auto Mode
- VPC with public/private subnets across multiple Availability Zones
- EKS node groups with optimized instance types
- AWS Load Balancer Controller and EBS CSI driver
- IAM roles and policies for secure AWS service integration

**Components We'll Deploy:**
- PostgreSQL database for Coder persistence
- Coder control plane services and web interface
- Network Load Balancer for external access
- CloudFront distribution for global content delivery
- Kubernetes Service accounts and additional IAM roles for workspace provisioning

## Key Benefits of This Architecture

- **High Availability**: Multi-AZ deployment ensures resilience
- **Auto Scaling**: EKS Auto Mode automatically manages capacity
- **Security**: Encrypted storage, private networking, and least-privilege IAM
- **Performance**: CloudFront CDN for optimal user experience globally
- **Cost Optimization**: Pay-per-use scaling and efficient resource utilization

{{% notice tip %}}
💡 **Pro Tip**: The EKS cluster uses Auto Mode, which automatically provisions and manages compute capacity, networking, and storage based on your workload requirements. This reduces operational overhead while optimizing costs.
{{% /notice %}}

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

### Ready to Get Started?
Let's begin by connecting to your EKS cluster and validating the infrastructure deployment.

