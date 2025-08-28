---
title: "Persona-Based Templates"
chapter: false
weight: 42
---

# Persona-Based Template Engineering

## Tailoring Environments to Developer Roles

Different developers have different needs. A frontend developer working on React applications requires different tools than a DevOps engineer managing Kubernetes clusters. Persona-based templates solve this by creating specialized environments optimized for specific roles and workflows.

This workshop leverages pre-built templates from the [AWS Coder Workshop GitOps repository](https://github.com/coder/aws-coder-workshop-gitops) that demonstrate production-ready persona-based development environments.

## Hands-On: Deploy Workshop Templates

In this section, you'll create a Kubernetes devcontainer workspace and use it to deploy the workshop templates to your Coder instance.

### Step 1: Create a Kubernetes Devcontainer Template

First, create a Kubernetes devcontainer template:

1. **Access your Coder dashboard, Templates** and click "New template"
2. **Select Kubernetes from the FILTER** and click "Use template" for the Kubernetes (Devcontainer) template
3. **Review Template configuration** and click "Save"

### Step 2: Create a Kubernetes Devcontainer Workspace

Next, create a workspace using the Kubernetes Devcontainer template for Coder administration:
1. **Access your Coder dashboard** and click "Create Workspace"
2. **Select the Kubernetes (Devcontainer) template** (created in the previous step)
3. **Configure the workspace parameters**:
   - **Name**: `template-admin-workspace`
   - **Repository**: `https://github.com/coder/aws-coder-workshop-gitops.git`
   - **CPU**: 2 cores
   - **Memory**: 4 GB
   - **Disk Size**: 20 GB

4. **Click "Create Workspace"** and wait for it to start

{{% notice info %}}
The devcontainer specification in the repository will automatically provision terraform, helm, kubectl, and other tools needed for template administration.
{{% /notice %}}

### Step 3: Access Your Template Administration Workspace

Once your workspace is running:

1. **Open the workspace** from your Coder dashboard
2. **Launch code-server** or your preferred editor
3. **Open a terminal** within the workspace

### Step 4: Initialize Template Deployment Environment

In your workspace terminal, set up the template deployment environment:

```bash
# Navigate to the templates directory
cd /workspaces/aws-coder-workshop-gitops/templates

# Verify required tools are available
terraform version
kubectl version --client
coder version

# Initialize Terraform
terraform init
```

### Step 4: Authenticate with Your Coder Instance

Authenticate the Coder CLI with your workshop instance:

```bash
# Login to your Coder instance 
coder login $CODER_AGENT_URL

# Verify authentication
coder whoami

# Create a session token for template deployment
CODER_SESSION_TOKEN=$(coder tokens create --lifetime 24h)
echo "Session token created successfully "+$CODER_SESSION_TOKEN
```

### Step 5: Deploy Workshop Templates

Use the provided GitOps script to deploy all workshop templates:

```bash
# Make the deployment script executable
chmod +x templates_gitops.sh

# Deploy all templates to your Coder instance
./templates_gitops.sh $CODER_SESSION_TOKEN
```

{{% notice tip %}}
The deployment script will create all 4 workshop templates in your Coder instance. When prompted by Terraform, respond "yes" to create the defined resources. This process typically takes 2-3 minutes.
{{% /notice %}}

### Step 6: Verify Template Deployment

Confirm that all templates were deployed successfully:

```bash
# List all available templates
coder templates list
```

You should see all 4 workshop templates listed and available for workspace creation.

### Step 7: Test Template Functionality

Create a test workspace to verify template functionality:

```bash
# Create a test workspace using the Kubernetes Q base template
coder create test-linux-q --template awshp-linux-q-base

# Check workspace status
coder list

# Clean up the test workspace
coder delete test-linux-q
```

{{% notice success %}}
Congratulations! You've successfully deployed all workshop templates using a GitOps workflow. Your Coder instance now has 4 persona-based templates ready for use.
{{% /notice %}}

## Next Steps

With persona-based templates deployed, you can now take a self-guided tour of their capabilities.