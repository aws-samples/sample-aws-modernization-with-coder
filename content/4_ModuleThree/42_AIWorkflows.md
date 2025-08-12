---
title: "AI-Driven Automation"
chapter: false
weight: 52
---

# AI-Driven Task Automation Workflows

## Transforming Development Tasks with Intelligent Automation

AI-driven automation workflows represent a potentially exponential boost to developer productivity by enabling Agentic AI to autonomously perform routine development tasks with minimul input and supervision. Instead of using precious Developer cycles, AI can automate standard SDLC tasks, allowing developers to focus on creative problem-solving and innovation.

### Workflow 2: AI-Driven Task Automation with Claude Code
#### Scenario: Perform an automated Well-Architected Review, and act on any findings

Let's create a Coder Task that will perform a Well-Architected review of the Web App we previously created.

#### Step 1: Create Your Task-Automation Workspace

Create a Task using the AWS Workshop - Kubernetes with Claude Code template:
1. **Access your Coder dashboard** and click "Tasks"
2. **Within the Task UI, Select the AWS Workshop - Kubernetes with Claude Code template from the drop-down** (created in the previous module)
3. **Configure the task prompt**:
   - "Analyze the Task Management Web App found at https://github.com/your-git-id/ai-dev-workflows.git, perform a Well Architected Review of the application with a focus on the Security pillar.  Create up to two additional Coder workspaces using the AWS Workshop - Kubernetes with Claude Code task template. Use the issues identified as the Task prompt for each additional workspace, and ensure the Task prompt for the new workspaces specifies that updates should be made as PRs to the original git repo at https://github.com/your-git-id/ai-dev-workflows.git"

4. **Click "Run task"** and wait for it to start

{{% notice info %}}
The Coder Task UI will automatically provision a task-based workspace and Claude Caude will be analyzing the provided Task Prompt.
{{% /notice %}}

The Claude Cod Web UI in the left pane will be begin by creating and updating a "To Do List" of activities to be peformed.  As is progresses, you can monitor the Agents actions.  It will most likely prompt for your approval and direction on how to move ahead to create new workspaces to resolve the identifid issues. Depending on how you respond, the Agent will spawn up to two additional Tasks to implemented to remediate the findings.

#### Step 2: Monitor and review Async Task-Automation Workspaces

Once your initial Task completes:

1. **Re-open the Tasks** from your Coder dashboard
2. **Review the new Tasks** created by your initial Task-Automation worflow
3. **Open the new tasks** and monitor the changes maded, and optionally submit PR's to your git repo


{{% notice tip %}}
🚀 *Workflow Optimization*: These AI workflows can reduce development time by 60-80% while improving code quality. Start with one workflow and gradually add more as your team becomes comfortable.
{{% /notice %}}

