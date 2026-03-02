---
title: "Persona-Based Template Engineering"
chapter: false
weight: 42
---

## Tailoring Environments to Developer Roles

Different developers have different needs. A frontend developer working on React applications requires different tools than a DevOps engineer managing Kubernetes clusters. Persona-based templates solve this by creating specialized environments optimized for specific roles and workflows.

This workshop leverages pre-built templates from the [AWS Coder Workshop GitOps repository](https://github.com/coder/aws-coder-workshop-gitops) that demonstrate production-ready persona-based development environments.

## Hands-On: Deploy Workshop Templates

In this section, you'll create a Kubernetes devcontainer workspace and use it to deploy the workshop templates to your Coder instance.

### Step 1: Create a Kubernetes Devcontainer Template

First, import a Kubernetes devcontainer template from the Coder Registry:

1. **Access your Coder dashboard, Workspaces** and select Kubernetes Workspace previously created
2. **Select code-server** to create a Code Server browser-based IDE session
3. **Select Terminal/New Terminal** within Code Server, and create a Terminal session
4. **Clone the Coder Registry** into your Workspace
```bash
git clone https://github.com/coder/registry.git

# Switch to the devcontainer template directory
cd registry/registry/coder/templates/kubernetes-devcontainer
```
5. **Import the Template** into your Coder Instance
```bash
# Login to your Coder instance 
coder login $CODER_AGENT_URL

# Verify authentication
coder whoami

# Push the contents of the template to your deployment
coder template push kubernetes-devcontainer -d .
```

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

::alert[The devcontainer specification in the repository will automatically provision terraform, helm, kubectl, and other tools needed for template administration.]{type="info"}

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

### Step 5: Authenticate with AWS Account/Workshop Studio Env 

Authenticate the Coder Workspace with your workshop AWS Account:
1. **Go to Workshop Instructions** AWS Account Access
2. **Select Get AWS CLI credentials** and Linux or macOS (bash)
![AWS CLI Credentials](/static/images/aws-cli-credentials.png)
3. **Copy content** and paste into Workspace terminal
4. **Validate AWS Configuration** using the CLI

```bash
# Verify AWS Credentials
aws sts get-caller-identity
```

### Step 6: Deploy Workshop Templates

Use the provided GitOps script to deploy all workshop templates:

```bash
# Make the deployment script executable
chmod +x templates_gitops.sh

# Deploy all templates to your Coder instance
./templates_gitops.sh $CODER_SESSION_TOKEN
```

::alert[The deployment script will create all 4 workshop templates in your Coder instance. When prompted by Terraform, respond "yes" to create the defined resources. This process typically takes 2-3 minutes.]{type="info"}

### Step 7: Verify Template Deployment

Confirm that all templates were deployed successfully:

```bash
# List all available templates
coder templates list
```

You should see all 4 workshop templates listed and available for workspace creation.

### Step 8: Test Template Functionality

Create a test workspace to verify template functionality:

```bash
# Create a test workspace using the Kubernetes Q base template
coder create test-linux-q --template awshp-linux-q-base

# Check workspace status
coder list

# Clean up the test workspace
coder delete test-linux-q
```


Congratulations! You've successfully deployed all workshop templates using a GitOps workflow. Your Coder instance now has 4 persona-based templates ready for use.


## Next Steps

With persona-based templates deployed, you can now take a self-guided tour of their capabilities.