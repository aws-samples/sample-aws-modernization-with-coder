---
title: "AI-Driven Development"
chapter: false
weight: 51
---

## Speed Up Development with Intelligent Automation

AI-driven development workflows represent a fundamental shift from reactive to proactive development. Instead of waiting for issues to arise, AI anticipates needs, suggests optimizations, and automates routine tasks, allowing developers to focus on creative problem-solving and innovation.

### Setting Up Your AI Development Environment

Let's create your Coder workspace with comprehensive AI development tools.

#### Step 1: Access Your AI-Enhanced Workspace

Create a workspace using the Kubernetes with Kiro CLI template:
1. **Access your Coder dashboard** and click "Create Workspace"
2. **Select the Kubernetes with Kiro CLI template** (pre-deployed in the workshop environment)
3. **Configure the workspace parameters**:
   - **Name**: `kiro-dev-workspace`
   - **CPU cores**: 2-4 vCPUs
   - **Memory**: 4-8 GiB RAM
   - **PVC storage size**: 30 GB (Default)

4. **Click "Create Workspace"** and wait for it to start

::alert[The Kubernetes with Kiro CLI template automatically provisions AWS CLI, AWS CDK, Node.js, and Kiro CLI with MCP server support for AI-driven AWS development.]{type="info"}

#### Step 2: Access Your Kiro Development Workspace

Once your workspace is running:

1. **Open the workspace** from your Coder dashboard
2. **Launch code-server** or Kiro IDE (if installed locally)
3. **Open a terminal** within the workspace

#### Step 3: Initialize Workshop Git/Github repo
Once in your workspace, let's create a workshop directory and initialize a git repo:
```bash
mkdir ai-dev-workflows 
```
Now from code-server or Kiro IDE:

1. **Use File/Open Folder** to open the workshop directory
2. **Use Git extension** to initialize a git repository in the current directory

#### Step 4: Initialize AI Development Tools
Back in your workspace terminal session, let's set up the AI development environment:
```bash
# Authenticate Kiro CLI (if not already done via the Kiro CLI app button)
kiro-cli login

# Start an AI chat session
kiro-cli chat
```

::alert[💡 **Kiro Advantage**: Kiro CLI supports Model Context Protocol (MCP) servers, enabling integration with tools like Pulumi, LaunchDarkly, and Arize for enhanced development workflows.]{type="info"}

### Workflow 1: AI-Assisted Feature Development
#### Scenario: Create a simple Cloud-Native Task Management Web App
Let's walk through developing a new feature using AI assistance from start to finish.

Step 1: Requirements Analysis with AI, start by describing your feature in natural language. Within the Kiro CLI chat session, provide the following prompt:
```bash
/plan
analyze the following requirements: "Create a simple task management web app that tracks task id, description, priority, and completion date. Provide two ways to interact with the data, one that summarizes open tasks by priority and another lists completed tasks by date". Create a technical requirements breakdown, provide architecture suggestions, implementation approaches, potential challenges and solutions for review.
```
Kiro will provide:
- Technical requirements breakdown
- Architecture suggestions
- Implementation approach
- Potential challenges and solutions

Step 2: AI-Generated Project Structure - have Kiro generate the supporting project structure with the following prompt:
```bash
Generate a supporting project structure for an AWS CDK application that uses typescript for the front end components and python for back-end API components that is deployable to AWS (e.g via CDK) and uses modern, cloud-native web development patterns.
```
Like any AI agent, Kiro may need additional guidance. If the agent isn't moving in the right direction, provide clarifying prompts to redirect its activity.  Kiro may prompt you for specific implementation decisions, and desired complexity of your initial application.

::alert[Kiro will request permission before creating or modifying files in your workspace, ensuring you maintain control over changes.]{type="info"}

This should create something similar to this with a corresponding task plan:
```bash
task-management-app/
├── infrastructure/          # AWS CDK TypeScript code
│   ├── bin/app.ts          # CDK app entry point
│   ├── lib/                # CDK stack definitions
│   │   ├── database-stack.ts    # DynamoDB table
│   │   ├── backend-stack.ts     # Lambda + API Gateway
│   │   └── frontend-stack.ts    # S3 + CloudFront
│   └── package.json        # CDK dependencies
├── backend/                # Python Lambda functions
│   ├── src/
│   │   ├── models/         # Data models
│   │   ├── services/       # Business logic
│   │   └── handlers/       # Lambda handlers
│   └── requirements.txt    # Python dependencies
├── frontend/               # React TypeScript app
│   ├── src/
│   │   ├── components/     # React components
│   │   ├── services/       # API client
│   │   └── types/          # TypeScript interfaces
│   └── package.json        # React dependencies
└── scripts/                # Deployment scripts
```

Step 3: AI-Generated Implementation and AWS Deployment - have Kiro complete the implementation an deploy the generated web app to the current AWS account:
```bash
Proceed with the defined plan and Smoke test the web app deployment to the current AWS account using the created deployment scripts
```
::alert[Kiro will first prompt you to switch from planning to execution mode. Kiro may then find and debug issues as it works with the existing scripts and workspace environment, installing required dependencies as needed. You'll see Kiro iterate across Lambda Functions, Back-End Schema, and other component issues as it tests the CDK stacks being deployed.]{type="info"}

When completed, at least your Database and Backend stacks should be successfully deployed and smoke-tested. You can continue to prompt Kiro to complete the full application deployment, if desired.

It is suggested you commit and push changes to your workshop Git repo at this point, as this Git repo will be used in the next AI-Driven Workflow example.

Step 4: Cleanup AI-Generated AWS Deployment - Have Kiro safely remove any deployments created for smoke-testing from the current AWS account:
```bash
Remove any CDK stack deployments used for smoke-testing the task management web app from the current AWS account. Double-check that only task management stacks are being deleted and nothing else.
```
This should remove any deployed components and ensure Kiro double-checks and reviews what was deleted. You can now end your Kiro CLI chat session with:
```bash
/quit
```

::alert[🚀 **Workflow Optimization**: These AI development workflows can reduce development time by 60-80% while improving code quality. Start with one workflow and gradually add more as your team becomes comfortable.]{type="info"}

### Kiro-Specific Capabilities

**MCP Server Integration**: Kiro's support for Model Context Protocol enables and related tools across a broad ecosystem of 3rd-Party services and AWS integrations.

**Workspace Trust**: Pre-configured workspace trust settings ensure seamless IDE integration without security prompts.

## Next Steps

Now that you've experimented with AI-Driven Development using Kiro, you can explore how AI-Driven Automation can further support your development workflow.
