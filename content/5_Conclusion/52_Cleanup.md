---
title: "Self-Hosted Workshop Cleanup"
chapter: false
weight: 62
---
# AWS Coder Workshop - Resource Cleanup Instructions

::alert[**Important**: Follow these cleanup steps if you ran this workshop in your own AWS account to avoid ongoing charges. If you used an AWS-provided sandbox environment, resources will be automatically cleaned up.]{type="warning"}

## Overview

This document provides step-by-step instructions to completely remove all AWS resources created during the AWS and Coder AI-Powered Development Workshop. The cleanup process should take approximately 15-30 minutes to complete.

## Prerequisites

- Access to the Coder dashboard
- Access to the AWS Management Console

## Cleanup Steps

### Step 1: Delete any Coder EC2 Workspaces

**Why**: Active EC2 workspaces consume compute resources and must be terminated before platform cleanup.

1. **Access your Coder dashboard** and click "Workspaces"
2. **Select individual Workspace and three dots on right** and click "Delete"
3. **Follow Workspace Delete Instructions** and repeat for all EC2 Workspaces

### Step 2: Delete core Workshop resources deployed via CloudFormation

**Why**: Clean-up core Amazon EKS and supporting Coder platform resources.

1. **Access your AWS Management Console, CloudFormation** and click "Stacks"
2. **Filter and select your Stack** and click the "Delete" button
3. **Follow Stack deletion instructions** and monitor until successfully complete

### Step 3: Delete individual IAM Role(s) created as part of workshop

**Why**: Clean up extraneous IAM Role(s) no longer needed.

1. **Access your AWS Management Console, IAM** and click "Roles"
2. **Search for "coder-workshop" roles** and select, then click the "Delete" button
3. **Follow IAM Role deletion instructions** and monitor until successfully complete
