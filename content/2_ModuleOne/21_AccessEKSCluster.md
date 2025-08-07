---
title: "Access Provisioned EKS Cluster" 
chapter: true
weight: 31 
---

# Access Provisioned EKS Cluster

## Connect to the Coder Control Plane Infrastructure
![AWS RefArch](/images/AWSCoderSingleRegionv1-1.png)

The core Coder Control Plane infrastructure has been automatically deployed using AWS EKS as depicted in the reference architecture above. The CloudFormation template has created all necessary foundational AWS services including VPC, subnets, compute resources, and storage, along with an EKS cluster named `coder-aws-cluster`.

Now we need to configure your local environment to connect to this pre-deployed cluster.

#### Step 1: Open AWS CloudShell
Open AWS CloudShell by navigating to: [https://console.aws.amazon.com/cloudshell](https://console.aws.amazon.com/cloudshell)

{{% notice info %}}
AWS CloudShell provides a browser-based shell with pre-installed AWS CLI, kubectl, and other tools needed for this workshop.
{{% /notice %}}

#### Step 2: Configure kubectl to Access Your EKS Cluster
Once AWS CloudShell is ready, run the following command to configure kubectl to connect to your EKS cluster:

```bash
# Connect to the pre-deployed EKS cluster
aws eks update-kubeconfig --name coder-aws-cluster --region us-east-1
```

#### Step 3: Validate EKS Cluster Access
Verify that you can successfully connect to the cluster and see the worker nodes:

```bash
# List the EKS nodes and validate their status as "Ready"
kubectl get nodes

# Get cluster information
kubectl cluster-info
```

You should see output showing your cluster nodes in a "Ready" state, confirming successful connection to the EKS cluster.

#### Step 4: Verify Storage Configuration
The EKS cluster has been configured with the AWS EBS CSI driver for persistent storage. You can verify the storage class configuration:

```bash
# Check available storage classes
kubectl get storageclass

# Verify the default storage class
kubectl get storageclass -o jsonpath='{.items[?(@.metadata.annotations.storageclass\.kubernetes\.io/is-default-class=="true")].metadata.name}'
```

{{% notice tip %}}
The cluster is configured with Auto Mode, which automatically manages compute capacity, networking, and storage based on your workload requirements. This simplifies cluster operations and optimizes costs.
{{% /notice %}}

### Next Steps
With your EKS cluster access configured, you're ready to deploy PostgreSQL and the Coder control plane in the following sections.
