---
title: "AI-Driven Task Automation"
chapter: false
weight: 52
---

## Transforming Development Tasks with Intelligent Automation

AI-driven automation workflows represent a potentially exponential boost to developer productivity by enabling Agentic AI to autonomously perform routine development tasks with minimul input and supervision. Instead of using precious Developer cycles, AI can automate standard SDLC tasks, allowing developers to focus on creative problem-solving and innovation.

### Workflow 2: AI-Driven Task Automation with Claude Code
#### Scenario: Perform an automated Well-Architected Review, and act on any findings

Let's create a Coder Task that will perform a Well-Architected review of the Web App we previously created.

#### Step 1: Create Your Task-Automation Workspace

Create a Task using the AWS Workshop - Kubernetes with Claude Code template:
1. **Access your Coder dashboard** and click "Tasks"
2. **Within the Task UI, Select the AWS Workshop - Kubernetes with Claude Code template from the drop-down** (created in the previous module)
3. **Configure the Task prompt**:
   - `Analyze the Task Management Web App found at https://github.com/your-git-id/ai-dev-workflows.git, perform a Well Architected Review of the application with a focus on the Security pillar.  Create up to two additional Coder workspaces using the AWS Workshop - Kubernetes with Claude Code task template. Use the issues identified as the Task prompt for each additional workspace, using the AI Prompt parameter. Ensure the Task prompt for the new workspaces specifies that updates should be made as PRs to the original git repo at https://github.com/your-git-id/ai-dev-workflows.git`

   - **note:** If you were not able to save your work from the prior workshop step, you can reference the following repo in the above prompt `https://github.com/greg-the-coder/ai-dev-workflows.git` 

4. **Click "Run task"** and wait for it to start

::alert[The Coder Task UI will automatically provision a task-based workspace and Claude Caude will begin analyzing the provided Task prompt.]{type="info"}

The Claude Cod Web UI in the left pane will be begin by creating and updating a "To Do List" of activities to be peformed.  As it progresses, you can monitor the Agents actions.  It will most likely prompt for your approval and direction on how to move ahead to create new workspaces to resolve the identified issues. Depending on how you respond, the Agent will spawn up to two additional Tasks to remediate the findings.

::alert[You may see some API Errors from Claude Code, due to Amazon Bedrock rate limiting.  Don't worry, one of the benefits of running asynchronsous Tasks using Autonomous Agents is that Claude Code will continue on until it completes the task or you stop it.]{type="info"}

![Codeer AI Driven Review](/static/images/ai-driven-app-review.png)

#### Step 2: Monitor and review Async Task-Automation Workspaces

Once your initial Task completes:

1. **Re-open the Tasks** from your Coder dashboard
2. **Review the new Tasks** created by your initial Task-Automation worflow
3. **Open the new tasks** and monitor the changes made, and optionally submit PR's to your git repo

![Codeer AI Driven Tasks](/static/images/ai-driven-task-automation.png)

Take some time on your own and explore each of the created Tasks and evaluate the results of the remediation activities performed by Claude Code.  Experiment by instructing Claude Code to unit test the changes and/or smoke-test a deployment to your AWS account.  

The prompts provided should work for the most part, but don't be surprised if Claude Code doesn't always act as directed.  You may need to stop the Agent(s) activities and re-direct with additional prompts or prompt the agent to continue or re-analyze it's activities if it get stuck or paused due to Bedrock rate limits on the Workshop accounts.

::alert[🚀 *Workflow Automation*: These AI automation workflows can reduce time spent on routine activities, while dramaticaly boosting developer productivity. Start by automating one workflow and experimenting until you get consistent results. Gradually add more AI automation workflows as your team becomes comfortable with specific Agent capabilities and required prompts to get the desired results.]{type="info"}

