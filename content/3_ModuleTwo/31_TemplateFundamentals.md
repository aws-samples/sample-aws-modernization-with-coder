---
title: "Template Fundamentals"
chapter: false
weight: 41
---

# Coder Template Fundamentals

## Understanding Template Architecture

Coder templates are the foundation of your development environment platform. They define how workspaces are provisioned, configured, and managed throughout their lifecycle. Think of templates as **Infrastructure as Code for development environments** - they combine Terraform's resource provisioning power with Coder's workspace management capabilities.

### Template Components

Every Coder template consists of several key components:

```
template/
├── main.tf              # Primary Terraform configuration
├── variables.tf         # Template parameters and inputs
├── outputs.tf           # Workspace connection details
├── startup_script.sh   # Workspace initialization script
├── README.md           # Template documentation
└── .coder/
    ├── img/             # Template icons and images
    └── apps/            # Application definitions
```

### Template Lifecycle

Understanding the template lifecycle is crucial for effective template engineering:

1. **Template Creation**: Define infrastructure resources and configuration
2. **Template Publishing**: Upload to Coder and make available to users
3. **Workspace Provisioning**: Users create workspaces from templates
4. **Workspace Management**: Start, stop, update, and delete operations
5. **Template Updates**: Version control and rolling updates

## Creating Your First Template

Let's start by creating a simple but functional template that demonstrates core concepts.

### Step 1: Access Your Coder Instance

First, let's connect to your Coder instance and set up the CLI:

```bash
# Get your Coder URL from the previous module
export CODER_URL="https://your-coder-instance.com"

# Create a session token (you'll do this through the web UI)
coder login $CODER_URL
```

### Step 2: Initialize a New Template

Create a new directory for your template:

```bash
mkdir ~/coder-templates
cd ~/coder-templates
mkdir basic-ubuntu-template
cd basic-ubuntu-template
```

### Step 3: Create the Main Terraform Configuration

Create `main.tf` with a basic Ubuntu workspace:

```hcl
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

# Coder provider configuration
provider "coder" {}

# AWS provider configuration
provider "aws" {
  region = var.aws_region
}

# Workspace metadata
data "coder_workspace" "me" {}
data "coder_workspace_owner" "me" {}

# EC2 instance for the workspace
resource "aws_instance" "workspace" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  
  vpc_security_group_ids = [aws_security_group.workspace.id]
  subnet_id              = var.subnet_id
  
  user_data = templatefile("${path.module}/startup_script.sh", {
    username = data.coder_workspace_owner.me.name
  })
  
  tags = {
    Name  = "coder-workspace-${data.coder_workspace_owner.me.name}-${data.coder_workspace.me.name}"
    Owner = data.coder_workspace_owner.me.name
    Coder = "true"
  }
}

# Ubuntu AMI data source
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-22.04-amd64-server-*"]
  }
  
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

# Security group for workspace
resource "aws_security_group" "workspace" {
  name_prefix = "coder-workspace-"
  description = "Security group for Coder workspace"
  
  # SSH access
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # HTTP for development servers
  ingress {
    from_port   = 8000
    to_port     = 8999
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # All outbound traffic
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "coder-workspace-sg"
  }
}

# Coder agent for workspace connection
resource "coder_agent" "main" {
  arch           = "amd64"
  os             = "linux"
  startup_script = file("${path.module}/startup_script.sh")
  
  # Metadata for workspace info
  metadata {
    display_name = "CPU Usage"
    key          = "cpu"
    script       = "top -bn1 | grep load | awk '{printf \"%.2f%%\", $(NF-2)}'"
    interval     = 10
    timeout      = 1
  }
  
  metadata {
    display_name = "Memory Usage"
    key          = "mem"
    script       = "free -m | awk 'NR==2{printf \"%.2f%%\", $3*100/$2 }'"
    interval     = 10
    timeout      = 1
  }
}

# VS Code Server application
resource "coder_app" "vscode" {
  agent_id     = coder_agent.main.id
  slug         = "vscode"
  display_name = "VS Code"
  icon         = "/icon/code.svg"
  url          = "http://localhost:8080"
  subdomain    = false
  share        = "owner"
  
  healthcheck {
    url       = "http://localhost:8080/healthz"
    interval  = 3
    threshold = 10
  }
}

# Terminal application
resource "coder_app" "terminal" {
  agent_id     = coder_agent.main.id
  slug         = "terminal"
  display_name = "Terminal"
  icon         = "/icon/terminal.svg"
  command      = "bash"
  share        = "owner"
}
```

### Step 4: Define Template Variables

Create `variables.tf` to make your template configurable:

```hcl
variable "aws_region" {
  description = "AWS region for workspace resources"
  type        = string
  default     = "us-west-2"
}

variable "instance_type" {
  description = "EC2 instance type for workspace"
  type        = string
  default     = "t3.medium"
  
  validation {
    condition = contains([
      "t3.micro", "t3.small", "t3.medium", "t3.large",
      "t3.xlarge", "t3.2xlarge"
    ], var.instance_type)
    error_message = "Instance type must be a valid t3 instance type."
  }
}

variable "subnet_id" {
  description = "Subnet ID for workspace placement"
  type        = string
}

variable "workspace_name" {
  description = "Name of the workspace"
  type        = string
  default     = "development"
}
```

### Step 5: Create the Startup Script

Create `startup_script.sh` to configure the workspace environment:

```bash
#!/bin/bash
set -euo pipefail

# Update system packages
sudo apt-get update
sudo apt-get upgrade -y

# Install essential development tools
sudo apt-get install -y \
    curl \
    wget \
    git \
    vim \
    htop \
    tree \
    jq \
    unzip \
    build-essential \
    software-properties-common

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ${username}

# Install Node.js (LTS)
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install Python and pip
sudo apt-get install -y python3 python3-pip python3-venv

# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
rm -rf aws awscliv2.zip

# Install VS Code Server
curl -fsSL https://code-server.dev/install.sh | sh

# Configure VS Code Server
mkdir -p ~/.config/code-server
cat > ~/.config/code-server/config.yaml << EOF
bind-addr: 0.0.0.0:8080
auth: none
password: ""
cert: false
EOF

# Install useful VS Code extensions
code-server --install-extension ms-python.python
code-server --install-extension ms-vscode.vscode-typescript-next
code-server --install-extension bradlc.vscode-tailwindcss
code-server --install-extension esbenp.prettier-vscode
code-server --install-extension ms-vscode.vscode-json

# Start VS Code Server
sudo systemctl enable --now code-server@${username}

# Create development directories
mkdir -p ~/projects
mkdir -p ~/scripts

# Set up Git configuration (users will customize this)
git config --global init.defaultBranch main
git config --global pull.rebase false

echo "Workspace setup complete! 🚀"
echo "Access VS Code at http://localhost:8080"
```

### Step 6: Define Outputs

Create `outputs.tf` to provide connection information:

```hcl
output "workspace_ip" {
  description = "Public IP address of the workspace"
  value       = aws_instance.workspace.public_ip
}

output "workspace_url" {
  description = "VS Code Server URL"
  value       = "http://${aws_instance.workspace.public_ip}:8080"
}

output "ssh_command" {
  description = "SSH command to connect to workspace"
  value       = "ssh ubuntu@${aws_instance.workspace.public_ip}"
}
```

## Template Best Practices

### 1. Resource Tagging
Always tag your AWS resources for cost tracking and management:

```hcl
tags = {
  Name        = "coder-workspace-${data.coder_workspace_owner.me.name}-${data.coder_workspace.me.name}"
  Owner       = data.coder_workspace_owner.me.name
  Environment = "development"
  Project     = data.coder_workspace.me.name
  ManagedBy   = "coder"
}
```

### 2. Security Groups
Implement least-privilege access:

```hcl
# Only allow necessary ports
ingress {
  from_port   = 22
  to_port     = 22
  protocol    = "tcp"
  cidr_blocks = [var.allowed_cidr_blocks]
}
```

### 3. Error Handling
Add validation and error handling:

```hcl
variable "instance_type" {
  validation {
    condition     = can(regex("^t3\\.(micro|small|medium|large|xlarge|2xlarge)$", var.instance_type))
    error_message = "Instance type must be a valid t3 instance type."
  }
}
```

### 4. Documentation
Include comprehensive README.md:

```markdown
# Basic Ubuntu Template

This template creates a Ubuntu 22.04 workspace with:
- VS Code Server
- Docker
- Node.js LTS
- Python 3
- AWS CLI

## Parameters
- `instance_type`: EC2 instance type (default: t3.medium)
- `aws_region`: AWS region (default: us-west-2)
- `subnet_id`: Subnet for workspace placement

## Applications
- **VS Code**: Web-based IDE at port 8080
- **Terminal**: Direct terminal access
```

## Publishing Your Template

Once your template is ready, publish it to Coder:

```bash
# Validate the template
terraform init
terraform validate

# Create the template in Coder
coder templates create basic-ubuntu \
  --directory . \
  --variable subnet_id="subnet-12345678"
```

{{% notice tip %}}
💡 **Pro Tip**: Start with a simple template and iterate. Add complexity gradually based on user feedback and requirements.
{{% /notice %}}

{{% notice info %}}
🔒 **Security Note**: Always follow the principle of least privilege when configuring security groups and IAM roles.
{{% /notice %}}

### Next Steps
Now that you understand template fundamentals, let's create persona-specific templates that cater to different developer roles and workflows.