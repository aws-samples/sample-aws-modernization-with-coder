---
title: "Persona-Based Templates"
chapter: false
weight: 42
---

# Persona-Based Template Engineering

## Tailoring Environments to Developer Roles

Different developers have different needs. A frontend developer working on React applications requires different tools than a DevOps engineer managing Kubernetes clusters. Persona-based templates solve this by creating specialized environments optimized for specific roles and workflows.

### Template Design Philosophy

Effective persona templates follow these principles:

- **Role-Specific Tooling**: Include only the tools and dependencies relevant to the persona
- **Optimized Performance**: Right-size resources for typical workloads
- **Workflow Integration**: Pre-configure common development workflows
- **Security by Default**: Implement role-appropriate security policies
- **Extensibility**: Allow customization without breaking core functionality

## Frontend Developer Template

### Overview
Optimized for modern web development with React, Vue, Angular, and other frontend frameworks.

### Key Features
- **Runtime**: Node.js 20+ with npm, yarn, and pnpm
- **Development Server**: Hot reload with port forwarding
- **Browser Tools**: Integrated browser testing and debugging
- **Build Tools**: Webpack, Vite, Parcel pre-configured
- **Testing**: Jest, Cypress, Playwright ready to use

### Template Implementation

Create `frontend-developer-template/main.tf`:

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

provider "coder" {}
provider "aws" {
  region = var.aws_region
}

data "coder_workspace" "me" {}
data "coder_workspace_owner" "me" {}

# Optimized instance for frontend development
resource "aws_instance" "frontend_workspace" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  
  vpc_security_group_ids = [aws_security_group.frontend_workspace.id]
  subnet_id              = var.subnet_id
  
  # Enhanced storage for node_modules and build artifacts
  root_block_device {
    volume_type = "gp3"
    volume_size = 50
    encrypted   = true
  }
  
  user_data = templatefile("${path.module}/frontend_startup.sh", {
    username = data.coder_workspace_owner.me.name
    node_version = var.node_version
  })
  
  tags = {
    Name     = "coder-frontend-${data.coder_workspace_owner.me.name}-${data.coder_workspace.me.name}"
    Owner    = data.coder_workspace_owner.me.name
    Persona  = "frontend-developer"
    Coder    = "true"
  }
}

# Security group optimized for frontend development
resource "aws_security_group" "frontend_workspace" {
  name_prefix = "coder-frontend-"
  description = "Security group for frontend development workspace"
  
  # SSH access
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # Development servers (React, Vue, Angular default ports)
  ingress {
    from_port   = 3000
    to_port     = 3000
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 4200
    to_port     = 4200
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 5173
    to_port     = 5173
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # Storybook default port
  ingress {
    from_port   = 6006
    to_port     = 6006
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # VS Code Server
  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "coder_agent" "frontend" {
  arch           = "amd64"
  os             = "linux"
  startup_script = file("${path.module}/frontend_startup.sh")
  
  metadata {
    display_name = "Node.js Version"
    key          = "node_version"
    script       = "node --version"
    interval     = 60
    timeout      = 5
  }
  
  metadata {
    display_name = "NPM Packages"
    key          = "npm_packages"
    script       = "find ~/projects -name package.json | wc -l"
    interval     = 300
    timeout      = 10
  }
}

# VS Code with frontend extensions
resource "coder_app" "vscode_frontend" {
  agent_id     = coder_agent.frontend.id
  slug         = "vscode"
  display_name = "VS Code (Frontend)"
  icon         = "/icon/code.svg"
  url          = "http://localhost:8080"
  subdomain    = false
  share        = "owner"
}

# React Dev Server
resource "coder_app" "react_dev" {
  agent_id     = coder_agent.frontend.id
  slug         = "react-dev"
  display_name = "React Dev Server"
  icon         = "/icon/react.svg"
  url          = "http://localhost:3000"
  subdomain    = true
  share        = "authenticated"
  
  healthcheck {
    url       = "http://localhost:3000"
    interval  = 10
    threshold = 3
  }
}

# Storybook
resource "coder_app" "storybook" {
  agent_id     = coder_agent.frontend.id
  slug         = "storybook"
  display_name = "Storybook"
  icon         = "/icon/storybook.svg"
  url          = "http://localhost:6006"
  subdomain    = true
  share        = "authenticated"
}
```

Create `frontend-developer-template/frontend_startup.sh`:

```bash
#!/bin/bash
set -euo pipefail

echo "Setting up Frontend Development Environment 🎨"

# Update system
sudo apt-get update && sudo apt-get upgrade -y

# Install essential tools
sudo apt-get install -y curl wget git vim htop tree jq unzip build-essential

# Install Node.js using NodeSource
curl -fsSL https://deb.nodesource.com/setup_${node_version}.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install global npm packages for frontend development
npm install -g \
  yarn \
  pnpm \
  @vue/cli \
  @angular/cli \
  create-react-app \
  vite \
  @storybook/cli \
  eslint \
  prettier \
  typescript \
  ts-node

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

# Install frontend-specific VS Code extensions
code-server --install-extension bradlc.vscode-tailwindcss
code-server --install-extension esbenp.prettier-vscode
code-server --install-extension dbaeumer.vscode-eslint
code-server --install-extension ms-vscode.vscode-typescript-next
code-server --install-extension ms-vscode.vscode-json
code-server --install-extension formulahendry.auto-rename-tag
code-server --install-extension christian-kohler.path-intellisense
code-server --install-extension ms-vscode.vscode-css-peek
code-server --install-extension bradlc.vscode-tailwindcss
code-server --install-extension steoates.autoimport-es6-ts

# Start VS Code Server
sudo systemctl enable --now code-server@${username}

# Create project structure
mkdir -p ~/projects/{react,vue,angular,vanilla}
mkdir -p ~/templates
mkdir -p ~/.npm-global

# Configure npm global directory
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc

# Create useful aliases
cat >> ~/.bashrc << 'EOF'

# Frontend Development Aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'

# Node.js aliases
alias ni='npm install'
alias nid='npm install --save-dev'
alias nig='npm install --global'
alias nt='npm test'
alias nr='npm run'
alias ns='npm start'
alias nb='npm run build'

# Yarn aliases
alias yi='yarn install'
alias ya='yarn add'
alias yad='yarn add --dev'
alias yt='yarn test'
alias yr='yarn run'
alias ys='yarn start'
alias yb='yarn build'

# Git aliases
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log --oneline'
alias gd='git diff'
EOF

# Install browser for testing (headless)
sudo apt-get install -y chromium-browser

# Create sample projects
cd ~/projects/react
npx create-react-app sample-app --template typescript

cd ~/projects/vue
npm create vue@latest sample-app -- --typescript --router --pinia --vitest --cypress

echo "Frontend Development Environment Ready! 🚀"
echo "Available tools:"
echo "  - Node.js $(node --version)"
echo "  - npm $(npm --version)"
echo "  - yarn $(yarn --version)"
echo "  - pnpm $(pnpm --version)"
echo "  - VS Code Server at http://localhost:8080"
echo "  - Sample React app in ~/projects/react/sample-app"
echo "  - Sample Vue app in ~/projects/vue/sample-app"
```

## Backend Developer Template

### Overview
Optimized for API development, microservices, and backend system development.

### Key Features
- **Multi-Language Support**: Go, Python, Java, Node.js, .NET
- **Database Tools**: PostgreSQL, MySQL, Redis clients
- **API Testing**: Postman CLI, curl, HTTPie
- **Monitoring**: Basic observability tools
- **Container Support**: Docker and Docker Compose

### Template Implementation

Create `backend-developer-template/main.tf`:

```hcl
# Backend-optimized instance with more CPU and memory
resource "aws_instance" "backend_workspace" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type # Default: t3.large
  
  vpc_security_group_ids = [aws_security_group.backend_workspace.id]
  subnet_id              = var.subnet_id
  
  # Enhanced storage for databases and build artifacts
  root_block_device {
    volume_type = "gp3"
    volume_size = 100
    encrypted   = true
    iops        = 3000
  }
  
  user_data = templatefile("${path.module}/backend_startup.sh", {
    username = data.coder_workspace_owner.me.name
  })
  
  tags = {
    Name     = "coder-backend-${data.coder_workspace_owner.me.name}-${data.coder_workspace.me.name}"
    Owner    = data.coder_workspace_owner.me.name
    Persona  = "backend-developer"
    Coder    = "true"
  }
}

# Security group for backend development
resource "aws_security_group" "backend_workspace" {
  name_prefix = "coder-backend-"
  description = "Security group for backend development workspace"
  
  # SSH access
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # API development ports
  ingress {
    from_port   = 8000
    to_port     = 8999
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # Database ports (PostgreSQL, MySQL, Redis)
  ingress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]
  }
  
  ingress {
    from_port   = 3306
    to_port     = 3306
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]
  }
  
  ingress {
    from_port   = 6379
    to_port     = 6379
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# API Documentation Server
resource "coder_app" "swagger_ui" {
  agent_id     = coder_agent.backend.id
  slug         = "swagger"
  display_name = "API Documentation"
  icon         = "/icon/swagger.svg"
  url          = "http://localhost:8080/docs"
  subdomain    = true
  share        = "authenticated"
}

# Database Admin Interface
resource "coder_app" "pgadmin" {
  agent_id     = coder_agent.backend.id
  slug         = "pgadmin"
  display_name = "Database Admin"
  icon         = "/icon/database.svg"
  url          = "http://localhost:5050"
  subdomain    = true
  share        = "owner"
}
```

## DevOps Engineer Template

### Overview
Optimized for infrastructure management, CI/CD, and platform engineering.

### Key Features
- **Infrastructure Tools**: Terraform, Ansible, Pulumi
- **Container Orchestration**: kubectl, helm, docker
- **Cloud CLIs**: AWS, Azure, GCP command-line tools
- **Monitoring**: Prometheus, Grafana, monitoring scripts
- **Security**: Security scanning and compliance tools

### Template Highlights

```hcl
# DevOps-optimized instance with enhanced networking
resource "aws_instance" "devops_workspace" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.xlarge" # More resources for infrastructure tasks
  
  # Enhanced IAM role for AWS operations
  iam_instance_profile = aws_iam_instance_profile.devops_profile.name
  
  vpc_security_group_ids = [aws_security_group.devops_workspace.id]
  subnet_id              = var.subnet_id
  
  user_data = templatefile("${path.module}/devops_startup.sh", {
    username = data.coder_workspace_owner.me.name
  })
}

# IAM role with necessary permissions for DevOps tasks
resource "aws_iam_role" "devops_role" {
  name = "coder-devops-role-${random_id.workspace.hex}"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      }
    ]
  })
}

# Kubernetes Dashboard
resource "coder_app" "k8s_dashboard" {
  agent_id     = coder_agent.devops.id
  slug         = "k8s-dashboard"
  display_name = "Kubernetes Dashboard"
  icon         = "/icon/kubernetes.svg"
  url          = "http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/"
  subdomain    = false
  share        = "owner"
}
```

## Data Science Template

### Overview
Optimized for machine learning, data analysis, and research workflows.

### Key Features
- **Python Ecosystem**: Jupyter, pandas, scikit-learn, TensorFlow, PyTorch
- **R Environment**: RStudio Server, tidyverse, data.table
- **GPU Support**: CUDA drivers for ML workloads
- **Data Storage**: Integration with S3, data lake access
- **Visualization**: Plotly, matplotlib, ggplot2

### Template Highlights

```hcl
# GPU-enabled instance for ML workloads
resource "aws_instance" "datascience_workspace" {
  ami           = data.aws_ami.deep_learning.id # Deep Learning AMI
  instance_type = var.gpu_instance_type # Default: g4dn.xlarge
  
  vpc_security_group_ids = [aws_security_group.datascience_workspace.id]
  subnet_id              = var.subnet_id
}

# Jupyter Lab
resource "coder_app" "jupyter" {
  agent_id     = coder_agent.datascience.id
  slug         = "jupyter"
  display_name = "Jupyter Lab"
  icon         = "/icon/jupyter.svg"
  url          = "http://localhost:8888"
  subdomain    = true
  share        = "owner"
}

# RStudio Server
resource "coder_app" "rstudio" {
  agent_id     = coder_agent.datascience.id
  slug         = "rstudio"
  display_name = "RStudio Server"
  icon         = "/icon/rstudio.svg"
  url          = "http://localhost:8787"
  subdomain    = true
  share        = "owner"
}
```

## Template Selection Strategy

### Automatic Template Recommendation

Implement logic to recommend templates based on:

1. **Project Repository Analysis**: Scan for package.json, requirements.txt, Dockerfile
2. **User Role**: Integration with identity providers and team assignments
3. **Resource Requirements**: CPU, memory, and storage needs
4. **Compliance Requirements**: Security and regulatory constraints

### Template Inheritance

Create a base template with common functionality:

```hcl
# base-template/main.tf
module "base_workspace" {
  source = "./modules/base-workspace"
  
  workspace_name = var.workspace_name
  owner_name     = data.coder_workspace_owner.me.name
  instance_type  = var.instance_type
  subnet_id      = var.subnet_id
}

# Extend with persona-specific features
module "frontend_extensions" {
  count  = var.persona == "frontend" ? 1 : 0
  source = "./modules/frontend-extensions"
  
  workspace_instance = module.base_workspace.instance
}
```

{{% notice tip %}}
💡 **Pro Tip**: Use template inheritance to maintain consistency while allowing specialization. This reduces duplication and makes updates easier.
{{% /notice %}}

{{% notice info %}}
🔄 **Continuous Improvement**: Regularly review template usage metrics and user feedback to optimize resource allocation and tool selection.
{{% /notice %}}

### Next Steps
Now let's integrate AI-powered development tools to supercharge these persona-based environments with intelligent assistance.