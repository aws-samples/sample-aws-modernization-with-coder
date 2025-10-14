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
    ├── provider         # Terraform resource providers
    ├── variable         # Terraform configuration variables
    ├── data             # Coder parameter and workspace configuration
    └── resource         # Coder agent and cloud infrastructure resource definition
├── README.md            # Template documentation
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

First, let's login to your Coder instance:
![Coder Login](/static/images/coder-login.png)

### Step 2: Initialize a New Template using an existing Starter Template

Select Templates then the New Template button:
![Coder Templates](/static/images/templates-listing.png)

From the Kubernetes starter templates, select the Kubernetes (Deployment) template:
![Coder K8S Starter Template](/static/images/kubernetes-deployment.png)

Finalize the template configuration and Save:
![Coder K8S Template Save](/static/images/k8s-template-save.png)

Update template variables, if prompted, and Retry:
![Coder K8S Template Variables](/static/images/k8s-template-variables.png)

Review the template main.tf (select Edit), then Create Workspace to provision your first Coder Workspace:
![Coder K8S Template Provision](/static/images/k8s-template-create.png)

Create your own unique Workspace name or use a suggestion, and then select Create Workspace
![Coder K8S Workspace Create](/static/images/K8s-workspace-create.png)

Your first Workspace is now successfully provisioned into your EKS Cluster as a Kubernetes Deployment (pod):
![Coder K8S Workspace Provisioned](/static/images/K8s-workspace-provisioned.png)

### Step 3: Validate Workspace connectivity via Terminal and code-server
Select Terminal to create an SSH connection to the Workspace, and enter a linux command to validate Workspace operation:
![Coder K8S Workspace Terminal](/static/images/k8s-workspace-terminal.png)

Select code-server to create a Code Server browser session, and explore the features of a browser-based VS Code compatible IDE:
![Coder K8S Workspace code-server](/static/images/k8s-workspace-codeserver.png)

**Optional**: Select VS Code Desktop if you have VS Code installed locally, and validate your Workspace connection.

::alert[💡 **Pro Tip**: Start with a simple template and iterate. Add complexity gradually based on user feedback and requirements.]{type="info"}

### Next Steps
Now that you understand template fundamentals, let's create persona-specific templates that cater to different developer roles and workflows.