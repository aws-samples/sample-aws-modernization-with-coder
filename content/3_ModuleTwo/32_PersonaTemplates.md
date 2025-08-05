---
title: "Persona-Based Templates"
chapter: false
weight: 42
---

# Persona-Based Template Engineering

## Tailoring Environments to Developer Roles

Different developers have different needs. A frontend developer working on React applications requires different tools than a DevOps engineer managing Kubernetes clusters. Persona-based templates solve this by creating specialized environments optimized for specific roles and workflows.

This workshop leverages pre-built templates from the [AWS Coder Workshop GitOps repository](https://github.com/coder/aws-coder-workshop-gitops) that demonstrate production-ready persona-based development environments.

## Available Workshop Templates

The workshop repository contains several specialized templates designed for different development personas and use cases:

### 1. AWS Linux Base with Amazon Q (`awshp-linux-q-base`)

**Purpose**: General-purpose Linux development environment with Amazon Q Developer integration

**Key Features**:
- AWS EC2 Linux instances with persistent storage
- Pre-installed Amazon Q Developer for AI-powered coding assistance
- VS Code Server with development extensions
- Git, Docker, and common development tools
- Optimized for cloud-native development workflows

**Use Cases**:
- General software development
- Cloud application development
- Learning and experimentation with Amazon Q
- Multi-language development projects

### 2. Kubernetes Development (`awshp-k8s-with-amazon-q`)

**Purpose**: Container-based development environment running on Kubernetes with Amazon Q integration

**Key Features**:
- Kubernetes pod-based workspaces for scalability
- Amazon Q Developer integration for container development
- Persistent home directory storage
- Pre-configured kubectl and container tools
- Ephemeral compute with persistent data

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

### 4. Kubernetes with Claude Code (`awshp-k8s-with-claude-code`)

**Purpose**: Container-based development environment with Claude Code AI assistant integration

**Key Features**:
- Kubernetes pod-based workspaces with AI assistance
- Claude Code AI assistant for automated development tasks
- Integration with AWS Bedrock for Claude models
- VS Code Web and Cursor AI-powered editor
- Configurable CPU, memory, and storage resources
- Built-in preview server for web applications

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
    Owner = data.coder_workspace_owner.me.name
    Coder_Provisioned = "true"
  }
}
```

### Security Best Practices

The templates implement several security best practices:

- **IAM Integration**: Proper AWS IAM roles and policies
- **Resource Tagging**: Consistent tagging for resource management
- **Network Security**: Appropriate security group configurations
- **Encryption**: EBS volume encryption by default
- **Access Control**: Workspace-level access controls

### Scalability Features

- **Auto-scaling**: Kubernetes-based templates support horizontal scaling
- **Resource Optimization**: Right-sized instances for different workloads
- **Persistent Storage**: Separation of compute and storage for cost efficiency
- **Multi-region Support**: Templates work across AWS regions

## GitOps Workflow Integration

The workshop templates demonstrate a complete GitOps workflow for template management:

### Template Versioning

Templates are managed through `template_versions.tf`:

```hcl
resource "coder_template" "awshp-linux-q-base" {
  name         = "awshp-linux-q-base"
  description  = "AWS EC2 Linux with Amazon Q Developer"
  organization_id = data.coder_organization.default.id
  
  versions = [{
    name      = "1.0.0"
    directory = "./awshp-linux-q-base"
    active    = true
  }]
}
```

### Deployment Automation

The repository includes `templates_gitops.sh` script for automated template deployment:

```bash
# Initialize Terraform
terraform init

# Login to Coder
coder login $CODER_AGENT_URL

# Deploy templates
./templates_gitops.sh <session-token>
```

## Implementing Templates in Your Workshop

### Step 1: Clone the Template Repository

```bash
# Clone the workshop templates
git clone https://github.com/coder/aws-coder-workshop-gitops.git
cd aws-coder-workshop-gitops/templates
```

### Step 2: Configure AWS Authentication

Ensure your Coder deployment has proper AWS credentials:

```bash
# Verify AWS access
aws sts get-caller-identity

# Check required permissions
aws iam simulate-principal-policy \
  --policy-source-arn $(aws sts get-caller-identity --query Arn --output text) \
  --action-names ec2:RunInstances ec2:DescribeInstances
```

### Step 3: Deploy Templates

```bash
# Initialize Terraform
terraform init

# Login to Coder
coder login https://your-coder-instance.com

# Deploy all templates
./templates_gitops.sh $(coder tokens create --lifetime 24h)
```

### Step 4: Verify Template Deployment

```bash
# List available templates
coder templates list

# Test template creation
coder create --template awshp-linux-q-base test-workspace
```

## Customizing Templates for Your Organization

### Adding Organization-Specific Tools

Extend the base templates with your organization's tools:

```hcl
# Add to startup_script in coder_agent resource
startup_script = <<-EOT
  #!/bin/bash
  
  # Base template setup
  ${file("${path.module}/base_setup.sh")}
  
  # Organization-specific tools
  curl -fsSL https://your-org.com/tools/install.sh | bash
  
  # Custom configurations
  cp /opt/your-org/configs/* ~/.config/
EOT
```

### Implementing Compliance Requirements

Add compliance and security configurations:

```hcl
# Enhanced security group
resource "aws_security_group" "workspace" {
  name_prefix = "coder-secure-"
  
  # Restrict SSH access to corporate network
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [var.corporate_cidr]
  }
  
  # Log all network traffic
  tags = {
    Compliance = "SOC2"
    Monitoring = "enabled"
  }
}
```

## Next Steps

With persona-based templates deployed, you can:

1. **Create Workspaces**: Users can create workspaces from available templates
2. **Monitor Usage**: Track template usage and performance metrics
3. **Iterate Templates**: Update templates based on user feedback
4. **Scale Deployment**: Add more specialized templates as needed

The templates from the workshop repository provide a solid foundation for building a comprehensive development platform that serves multiple developer personas while maintaining consistency and security standards.