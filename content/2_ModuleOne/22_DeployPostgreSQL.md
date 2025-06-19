---
title: "Deploy PostgreSQL to store Backend State" 
chapter: true
weight: 32 
---

# Deploy PostgreSQL to store Backend State 

## Create Database Instance for Coder Control Plane 

The Coder Control Plane uses a PostgreSQL compatible database to store backend state.  As part of this workshop, we will be using a simplified deployment suitable for POC's and testing, and deploy PostgreSQL into our EKS Cluster using Helm.

From AWS CloudShell and in the AWS account/region being used for the workshop, perform the following steps:

#### Step 1: Create a Kubernetes Namespace for the Coder Control Plane
```bash
# Create Coder namespace
kubectl create namespace coder
```

#### Step 2: Create Coder PostgreSQL DB Instance using Helm
```bash
# Install Bitnami PostgreSQL using Helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install coder-db bitnami/postgresql \
    --namespace coder \
    --set auth.username=coder \
    --set auth.password=coder \
    --set auth.database=coder \
    --set persistence.size=10Gi
```

#### Step 3: Store Coder PostgreSQL DB Configuration as Secret
```bash
# Create database connection secret for Coder
kubectl create secret generic coder-db-url -n coder \
  --from-literal=url="postgres://coder:coder@coder-db-postgresql.coder.svc.cluster.local:5432/coder?sslmode=disable"
```

Tip:  Check the state of the coder-db-postgresql-(instance) pod created in the coder namespace to ensure it's running, and pod logs to ensure it deployed successfully.  Common trouble-shooting issues arise around Kubernetes/EKS PVC and PV creation and allocation.  Start by validating that the required storage (PV/PVC) is being created successfully. 

### Next Section Heading <!-- MODIFY THIS HEADING -->
This paragraph block can optionally be utilized to lead into the next section of the workshop.
