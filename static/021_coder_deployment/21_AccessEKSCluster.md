---
title: "Access Provisioned EKS Cluster" 
chapter: false
weight: 31 
---

## Connect to the Coder Control Plane Infrastructure
![AWS RefArch](https://raw.githubusercontent.com/coder/coder/main/docs/images/architecture-diagram.png)

The core Coder Control Plane infrastructure has been automatically deployed using AWS EKS as depicted in the reference architecture above. The CloudFormation template has created all necessary foundational AWS services including VPC, subnets, compute resources, and storage, along with an EKS cluster named `coder-aws-cluster`.

Now we need to configure your local environment to connect to this pre-deployed cluster.

#### Step 1: Open AWS CloudShell
Open AWS CloudShell by navigating to: [https://console.aws.amazon.com/cloudshell](https://console.aws.amazon.com/cloudshell)

::alert[AWS CloudShell provides a browser-based shell with pre-installed AWS CLI, kubectl, and other tools needed for this workshop.]{type="info"}

#### Step 2: Configure kubectl to Access Your EKS Cluster
Once AWS CloudShell is ready, run the following command to configure kubectl to connect to your EKS cluster:

```bash
# Connect to the pre-deployed EKS cluster
aws eks update-kubeconfig --name coder-aws-cluster --region <workshop region>
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

#### Step 4: Update and Verify Storage Configuration
The EKS cluster needs to be configured with a default AWS EBS CSI driver that supports dynamic provisioning of persistent storage. 

```bash
# Deploy a K8S StorageClass for dynamic EBS volume provisioning
kubectl apply -f - <<EOF
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp3-csi
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: ebs.csi.eks.amazonaws.com
volumeBindingMode: WaitForFirstConsumer
parameters:
  type: gp3
  encrypted: "true"
allowVolumeExpansion: true
EOF
```
You can verify the storage class configuration:

```bash
# Check available storage classes
kubectl get storageclass

# Verify the default storage class
kubectl get storageclass -o jsonpath='{.items[?(@.metadata.annotations.storageclass\.kubernetes\.io/is-default-class=="true")].metadata.name}'
```

### Next Steps
With your EKS cluster access configured, you're ready to deploy PostgreSQL and the Coder control plane in the following sections.
