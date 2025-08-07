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
   - **Repository**: `https://github.com/coder/aws-coder-workshop-gitops`
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
2. **Launch VS Code** or your preferred editor
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
# Login to your Coder instance (replace with your actual URL)
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
The deployment script will create all 5 workshop templates in your Coder instance. This process typically takes 2-3 minutes.
{{% /notice %}}

### Step 6: Verify Template Deployment

Confirm that all templates were deployed successfully:

```bash
# List all available templates
coder templates list
```

You should see all 5 workshop templates listed and available for workspace creation.

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
Congratulations! You've successfully deployed all workshop templates using a GitOps workflow. Your Coder instance now has 5 persona-based templates ready for use.
{{% /notice %}}

## Available Workshop Templates

The workshop repository contains several specialized templates designed for different development personas and use cases:

### 1. AWS Linux Base with Amazon Q (`awshp-linux-q-base`)

**Purpose**: General-purpose Linux development environment with Amazon Q Developer CLI integration

**Key Features**:
- AWS EC2 Linux instances with persistent storage
- Pre-installed Amazon Q Developer CLI for AI-powered coding assistance
- VS Code Server with development extensions
- Git, Docker, and common development tools
- Optimized for cloud-native development workflows

**Use Cases**:
- General software development
- Cloud application development
- Learning and experimentation with Amazon Q
- Multi-language development projects

### 2. Kubernetes Development with Amazon Q (`awshp-k8s-with-amazon-q`)

**Purpose**: Container-based development environment running on Kubernetes with Amazon Q Agentic Task integration

**Key Features**:
- Kubernetes pod-based workspaces for scalability
- Amazon Q Developer CLI integration for automated development tasks
- Persistent home directory storage
- Pre-configured kubectl and container tools
- Ephemeral compute with persistent data
- Coder Tasks integration, Run Async Agentic AI Tasks in Coder Workspaces

**Use Cases**:
- Microservices development
- Kubernetes application development
- Container orchestration learning
- Cloud-native architecture development

### 3. AWS SAM Development (`awshp-linux-sam`)

**Purpose**: Serverless application development with AWS SAM (Serverless Application Model)

**Key Features**:
- AWS EC2 Linux instances optimized for serverless development
- AWS SAM CLI pre-installed and configured
- Lambda development tools and runtime environments
- AWS CLI with proper IAM integration
- Code deployment and testing capabilities

**Use Cases**:
- AWS Lambda function development
- Serverless application architecture
- Event-driven application development
- AWS service integration projects

### 4. Kubernetes Development with Claude Code (`awshp-k8s-with-claude-code`)

**Purpose**: Container-based development environment running on Kubernetes with Claude Code Agentic Task integration

**Key Features**:
- Kubernetes pod-based workspaces with AI assistance
- Claude Code AI assistant for automated development tasks
- Integration with AWS Bedrock for Claude models
- VS Code Web and Cursor AI-powered editor
- Configurable CPU, memory, and storage resources
- Coder Tasks integration, Run Async Agentic AI Tasks in Coder Workspaces

**Use Cases**:
- AI-assisted software development
- Rapid prototyping with AI code generation
- Learning and experimentation with AI coding tools
- Container-based development with intelligent assistance

### 5. Windows Development with DCV (`awshp-windows-dcv`)

**Purpose**: Windows-based development environment with NICE DCV remote desktop

**Key Features**:
- Windows Server 2022 EC2 instances
- NICE DCV for high-performance remote desktop access
- VS Code on Windows with full GUI support
- PowerShell and Windows development tools
- Configurable instance types and storage

**Use Cases**:
- .NET application development
- Windows-specific software development
- Legacy application modernization
- Cross-platform development testing

## Template Architecture Patterns

### Infrastructure as Code Approach

All templates follow Terraform-based Infrastructure as Code principles:

```hcl
# Example template structure from the repository
terraform {
  required_providers {
    coder = {
      source = "coder/coder"
    }
    aws = {
      source = "hashicorp/aws"
    }
  }
}

# Workspace and owner data sources
data "coder_workspace" "me" {}
data "coder_workspace_owner" "me" {}

# AWS resources with proper tagging
resource "aws_instance" "workspace" {
  # Instance configuration
  tags = {
    Name = "coder-${data.coder_workspace_owner.me.name}-${data.coder_workspace.me.name}"
    Coder_Provisioned = "true"
  }
}
```

### Security Best Practices

The templates implement several security best practices:

- **IAM Integration**: Proper AWS IAM roles and policies
- **Resource Tagging**: Consistent tagging for resource management
- **Encryption**: EBS volume encryption by default
- **Access Control**: Workspace-level access controls

### Scalability Features

- **Auto-scaling**: Kubernetes-based templates support horizontal scaling of EKS Cluster Node Pools
- **Resource Optimization**: Right-sized instances for different workloads
- **Persistent Storage**: Separation of compute and storage for cost efficiency
- **Multi-region Support**: Templates work across AWS regions

## Template Management and Updates

Now that you've deployed the workshop templates, you can manage and update them using the same GitOps workflow:

### Updating Templates

To update templates with new features or configurations:

```bash
# In your template-admin-workspace
cd /workspaces/aws-coder-workshop-gitops/templates

# Make changes to template files
# Edit main.tf files in template directories

# Deploy updates
./templates_gitops.sh $CODER_SESSION_TOKEN
```

### Adding New Templates

To add organization-specific templates:

```bash
# Create new template directory
mkdir awshp-custom-template
cd awshp-custom-template

# Create template files
# Add main.tf, README.md, and other required files

# Update template_versions.tf to include new template
# Deploy changes
cd ..
./templates_gitops.sh $CODER_SESSION_TOKEN
```

## Next Steps

With persona-based templates deployed, you can:

1. **Create Workspaces**: Users can create workspaces from available templates
2. **Monitor Usage**: Track template usage and performance metrics
3. **Iterate Templates**: Update templates based on user feedback
4. **Scale Deployment**: Add more specialized templates as needed

The templates from the workshop repository provide a solid foundation for building a comprehensive development platform that serves multiple developer personas while maintaining consistency and security standards.