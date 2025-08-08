---
title: "Explore Available Templates"
chapter: false
weight: 43
---

# Explore Available Template Capabilities

## Available Workshop Templates

The workshop repository contains several specialized templates designed for different development personas and use cases.  Spend some time on a self-guided tour of their capabiities by creating workspaces for each of the workshop templates provisioned:

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

### 2. AWS SAM Development (`awshp-linux-sam`)

**Purpose**: Serverless application development with AWS SAM (Serverless Application Model)

**Key Features**:
- AWS EC2 ARM64/Graviton Linux instances optimized for serverless development
- AWS SAM CLI pre-installed and configured
- AWS CLI with proper IAM integration
- Amazon Q IDE Extension pre-installed
- Lambda development tools and runtime environments
- Code deployment and testing capabilities

**Use Cases**:
- AWS Lambda function development
- Serverless application architecture
- Event-driven application development
- AWS service integration projects

### 3. Kubernetes Development with Claude Code (`awshp-k8s-with-claude-code`)

**Purpose**: Container-based development environment running on Kubernetes with Claude Code Agentic Task integration

**Key Features**:
- Kubernetes pod-based workspaces with AI assistance
- Claude Code AI assistant for automated development tasks
- Integration with AWS Bedrock for Claude models
- VS Code Web and Kiro AI-powered editor
- Configurable CPU, memory, and storage resources
- Coder Tasks integration, Run Async Agentic AI Tasks in Coder Workspaces

**Use Cases**:
- AI-assisted software development
- Rapid prototyping with AI code generation
- Learning and experimentation with AI coding tools
- Container-based development with intelligent assistance

### 4. Windows Development with DCV (`awshp-windows-dcv`)

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