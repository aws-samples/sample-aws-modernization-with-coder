---
title: "Explore Available Template Capabilities"
chapter: false
weight: 43
---

## Available Workshop Templates

The workshop environment includes five pre-deployed templates designed for different development personas and use cases. Explore their capabilities by creating workspaces from each template:

### 1. Kubernetes with Kiro CLI (`awshp-k8s-with-kiro-cli`)

**Purpose**: AI-powered development with Kiro CLI and MCP server support

**Key Features**:
- Kubernetes pod-based workspaces with persistent storage
- Kiro CLI with Model Context Protocol (MCP) server support
- Pre-installed AWS CLI v2, AWS CDK, and Node.js 20 LTS
- uv/uvx for Python-based MCP servers
- Workspace trust pre-configured for seamless IDE integration
- Fast startup times (~30-60 seconds)

**Use Cases**:
- AI-assisted development with configurable MCP servers
- Infrastructure as Code with AWS CDK
- General-purpose AWS development on Kubernetes
- MCP server integration (Pulumi, LaunchDarkly, Arize, etc.)

### 2. Kubernetes with Claude Code (`awshp-k8s-with-claude-code`)

**Purpose**: Container-based development with Claude Code 4.7.1 AI assistant and task automation

**Key Features**:
- Kubernetes pod-based workspaces with AI assistance
- Claude Code 4.7.1 with AWS Bedrock (Claude Opus 4.5)
- Task-based workflows using `coder task create`
- Pre-installed AWS CLI, AWS CDK, Node.js 20.x
- Preview server on port 3000 with health checks
- Automated testing with Playwright integration

**Use Cases**:
- AI-driven development with task-based workflows
- Rapid prototyping with AI code generation
- Microservices and container orchestration
- Automated testing and deployment

### 3. Kubernetes RAG with Claude Code (`awshp-k8s-rag-with-claude-code`)

**Purpose**: RAG application prototyping with Aurora PostgreSQL pgvector and Claude Code

**Key Features**:
- Kubernetes pod-based workspaces optimized for RAG development
- Aurora PostgreSQL 16.8 Serverless v2 with pgvector extension
- Claude Code 4.7.1 with AWS Bedrock (Claude Sonnet 4.5)
- Streamlit preview on port 8501
- Auto-configured database with environment variables
- Git repository auto-cloning for RAG prototyping

**Use Cases**:
- AI/ML application development with RAG patterns
- Vector database integration for embeddings
- Document processing and intelligent search systems
- Streamlit-based data applications

### 4. AWS SAM Development (`awshp-linux-sam`)

**Purpose**: Serverless application development with AWS SAM CLI on ARM64

**Key Features**:
- Amazon EC2 ARM64/Graviton instances for cost-effective development
- AWS Serverless Application Model (SAM) CLI pre-installed
- AWS CLI v2 with ARM64 optimization
- Python 3 runtime environment
- Local Lambda and API Gateway testing

**Use Cases**:
- AWS Lambda function development
- Serverless application architecture
- Cost-effective ARM64 development
- Local serverless testing

### 5. Windows Development with DCV (`awshp-windows-dcv`)

**Purpose**: Windows-based development with NICE DCV remote desktop

**Key Features**:
- Windows Server 2022 Amazon EC2 instances
- NICE DCV for high-performance remote desktop access
- VS Code on Windows with full GUI support
- PowerShell and Windows development tools
- Multi-region support (15 AWS regions)

**Use Cases**:
- .NET application development
- Windows-specific software development
- GUI application development
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

The templates are pre-deployed in your workshop environment. You can manage and update them using the GitOps workflow:

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

With pre-deployed templates available, you can:

1. **Create Workspaces**: Users can create workspaces from available templates
2. **Monitor Usage**: Track template usage and performance metrics
3. **Iterate Templates**: Update templates based on user feedback
4. **Scale Deployment**: Add more specialized templates as needed

The pre-deployed templates provide a solid foundation for building a comprehensive development platform that serves multiple developer personas while maintaining consistency and security standards.