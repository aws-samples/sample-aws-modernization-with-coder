---
title: Enable Amazon Bedrock Access
weight: 23
---

# Enable Amazon Bedrock Models in the AWS Console

Before Coder Cloud CDE Workspaces can interact with Amazon Bedrock/Claude Code, we need to enable access to the supporting foundation models (FMs) used during the workshop.

> 📍 **Prerequisite:** You should have already deployed the CloudFormation template and obtained your IAM credentials.

---

## Step 1: Sign in to the AWS Console


2. Make sure you're in the **N. Virginia (us-east-1)** region — this is where Amazon Bedrock is fully supported.

---

## Step 2: Open Amazon Bedrock

1. [Click here](https://console.aws.amazon.com/bedrock/home) or within the **Search bar** at the top of the AWS Console, type **"Bedrock"** and click on **Amazon Bedrock** from the dropdown.

2. If prompted to enable the service, click **Enable access**.

---

## Step 3: Enable Foundation Model Access

1. In the left navigation menu, click **Model access**.
2. Click the **Modify model access** button.
3. Select the following models (these can be used for this workshop):
   - **Anthropic Claude 3.5 Haiku**
   - **Anthropic Claude Sonnet 4**
   - **Anthropic Claude 3.5 Sonnet v2**
   - **Anthropic Claude 3.7 Sonnet**

4. Click **Next**, then **Submit** to confirm model access.
5. Wait for the access to be granted — this may take a few minutes.

---

## Step 4: Verify Access

After a few minutes, return to the **Model access** page and ensure the status for each selected model says **Access granted**.