---
title: "Create IAM/EC2 Control Plane Role" 
chapter: true
weight: 34 
---

# Create IAM/EC2 Control Plane Role 

## Create IAM Role for EC2 VM Coder Workspaces  
{{% notice info %}}
**Workshop Author Note**:  Need to update this section to create a role that can also be used later for an IAM Instance Profile in the EC2-Based Workspace Templates 
{{% /notice %}}

The Coder Control Plane requires additional permissions to create stand-alone AWS EC2 VM-Based Coder Workspaces from supporting Coder Templates.  AWS EC2 VM-based Workspaces will still be orchestrated by the core Coder Control Plane on EKS, and will use EKS pod-identity to associate the necessary IAM role.

From AWS CloudShell and in the AWS account/region being used for the workshop, perform the following steps:

#### Step 1: Create IAM Role and Trust Relationship for EC2 Workspace Support
```bash
# Make sure you have the ekspodid-trust-policy.json file in your current directory (update role name)
aws iam create-role --role-name your-coder-ec2-workspace-role --assume-role-policy-document file://ekspodid-trust-policy.json

# Attach necessary policies to the role 
aws iam attach-role-policy \
    --role-name your-coder-ec2-workspace-role \
    --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess

aws iam attach-role-policy \
    --role-name your-coder-ec2-workspace-role \
    --policy-arn arn:aws:iam::aws:policy/IAMReadOnlyAccess
```

#### Step 2: Associate IAM Role with EKS/Coder Control Plane
```bash
# Add IAM Pod Identity association for EC2 Workspace support
aws eks create-pod-identity-association \
    --cluster-name your-cluster-name \
    --namespace coder \
    --service-account coder \
    --role-arn arn:aws:iam::your-aws-account-id:role/your-coder-ec2-workspace-role
```
{{% notice info %}}
The updated IAM Role association may not take affect until the Coder Control Plane is restarted.  Delete the coder-(instance) pods in the coder namespace that are currently running, and validate that new ones are automatically started and running. 
{{% /notice %}}

### Next Steps <!-- MODIFY THIS HEADING -->
With the required IAM Role successfully created and associated with the Coder EKS Service Account, you're ready to create a CloudFront distribution for the Coder Control Plane.