---
title: "AI-Powered Development"
chapter: false
weight: 43
---

# AI-Powered Development Integration

## Supercharging Development with AWS AI Services

Modern development environments aren't just about tools and infrastructure—they're about intelligent assistance that understands your code, suggests improvements, and automates repetitive tasks. In this section, we'll integrate AWS AI services to create truly intelligent development environments.

### AI Services Integration Overview

We'll integrate several AWS AI services into our Coder templates:

- **Amazon Q Developer**: Intelligent code completion and generation
- **AWS Bedrock with Claude**: Advanced code review and documentation
- **Amazon CodeGuru**: Automated code review and performance recommendations
- **AWS CodeWhisperer**: Real-time code suggestions and security scanning

## Amazon Q Developer Integration

### Overview
Amazon Q Developer provides intelligent code assistance directly in your IDE, understanding your codebase context and providing relevant suggestions.

### Template Integration

Add Amazon Q Developer to your templates by modifying the startup script:

```bash
# Install Amazon Q Developer CLI
curl -o q-developer-cli.tar.gz https://d2yblsmsllhwdu.cloudfront.net/q-developer-cli/latest/q-developer-cli-linux-x64.tar.gz
tar -xzf q-developer-cli.tar.gz
sudo mv q-developer-cli /usr/local/bin/q
rm q-developer-cli.tar.gz

# Configure Q Developer
q configure --region ${aws_region}

# Install VS Code extension for Q Developer
code-server --install-extension amazonwebservices.amazon-q-vscode
```

### Terraform Configuration for Q Developer

```hcl
# IAM role for Q Developer access
resource "aws_iam_role" "q_developer_role" {
  name = "coder-q-developer-${random_id.workspace.hex}"
  
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

# Policy for Q Developer
resource "aws_iam_role_policy" "q_developer_policy" {
  name = "q-developer-policy"
  role = aws_iam_role.q_developer_role.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "q:*",
          "codewhisperer:*",
          "codeguru-reviewer:*"
        ]
        Resource = "*"
      }
    ]
  })
}

# Instance profile
resource "aws_iam_instance_profile" "q_developer_profile" {
  name = "q-developer-profile-${random_id.workspace.hex}"
  role = aws_iam_role.q_developer_role.name
}
```

### Q Developer Configuration Script

Create `scripts/configure_q_developer.sh`:

```bash
#!/bin/bash
set -euo pipefail

echo "Configuring Amazon Q Developer 🤖"

# Create Q Developer configuration
mkdir -p ~/.aws/q-developer

cat > ~/.aws/q-developer/config.json << EOF
{
  "region": "${aws_region}",
  "enableCodeSuggestions": true,
  "enableSecurityScanning": true,
  "enableCodeReview": true,
  "autoAcceptSuggestions": false,
  "logLevel": "info"
}
EOF

# Configure VS Code settings for Q Developer
mkdir -p ~/.local/share/code-server/User
cat > ~/.local/share/code-server/User/settings.json << EOF
{
  "amazonQ.enableCodeSuggestions": true,
  "amazonQ.enableSecurityScanning": true,
  "amazonQ.shareCodewhispererContentWithAWS": true,
  "amazonQ.telemetry": true,
  "editor.inlineSuggest.enabled": true,
  "editor.suggest.preview": true
}
EOF

echo "Amazon Q Developer configured successfully!"
```

## AWS Bedrock with Claude Integration

### Overview
Integrate AWS Bedrock with Anthropic's Claude for advanced code review, documentation generation, and architectural guidance.

### Bedrock Template Configuration

```hcl
# IAM role for Bedrock access
resource "aws_iam_role" "bedrock_role" {
  name = "coder-bedrock-${random_id.workspace.hex}"
  
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

# Bedrock access policy
resource "aws_iam_role_policy" "bedrock_policy" {
  name = "bedrock-policy"
  role = aws_iam_role.bedrock_role.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "bedrock:InvokeModel",
          "bedrock:InvokeModelWithResponseStream"
        ]
        Resource = [
          "arn:aws:bedrock:*::foundation-model/anthropic.claude-3-sonnet-20240229-v1:0",
          "arn:aws:bedrock:*::foundation-model/anthropic.claude-3-haiku-20240307-v1:0"
        ]
      }
    ]
  })
}
```

### Claude Code Assistant Script

Create `scripts/claude_assistant.py`:

```python
#!/usr/bin/env python3
import boto3
import json
import sys
import argparse
from pathlib import Path

class ClaudeCodeAssistant:
    def __init__(self, region='us-west-2'):
        self.bedrock = boto3.client('bedrock-runtime', region_name=region)
        self.model_id = 'anthropic.claude-3-sonnet-20240229-v1:0'
    
    def review_code(self, file_path, context=""):
        """Review code file and provide suggestions"""
        try:
            with open(file_path, 'r') as f:
                code_content = f.read()
        except FileNotFoundError:
            return f"Error: File {file_path} not found"
        
        prompt = f"""
Please review the following code and provide:
1. Code quality assessment
2. Security vulnerabilities
3. Performance improvements
4. Best practice recommendations
5. Refactoring suggestions

Context: {context}

Code:
```
{code_content}
```

Provide a structured review with specific, actionable feedback.
"""
        
        return self._invoke_claude(prompt)
    
    def generate_documentation(self, file_path):
        """Generate documentation for code file"""
        try:
            with open(file_path, 'r') as f:
                code_content = f.read()
        except FileNotFoundError:
            return f"Error: File {file_path} not found"
        
        prompt = f"""
Generate comprehensive documentation for the following code:

```
{code_content}
```

Include:
1. Overview and purpose
2. Function/class descriptions
3. Parameter documentation
4. Return value descriptions
5. Usage examples
6. Dependencies and requirements

Format as markdown.
"""
        
        return self._invoke_claude(prompt)
    
    def explain_code(self, file_path, specific_function=None):
        """Explain code functionality"""
        try:
            with open(file_path, 'r') as f:
                code_content = f.read()
        except FileNotFoundError:
            return f"Error: File {file_path} not found"
        
        focus = f"Focus on the function: {specific_function}" if specific_function else "Explain the entire codebase"
        
        prompt = f"""
{focus}

Code:
```
{code_content}
```

Provide a clear, beginner-friendly explanation of:
1. What the code does
2. How it works
3. Key algorithms or patterns used
4. Potential edge cases
5. How to use or extend it
"""
        
        return self._invoke_claude(prompt)
    
    def _invoke_claude(self, prompt):
        """Invoke Claude model with prompt"""
        try:
            body = json.dumps({
                "anthropic_version": "bedrock-2023-05-31",
                "max_tokens": 4000,
                "messages": [
                    {
                        "role": "user",
                        "content": prompt
                    }
                ]
            })
            
            response = self.bedrock.invoke_model(
                modelId=self.model_id,
                body=body
            )
            
            response_body = json.loads(response['body'].read())
            return response_body['content'][0]['text']
            
        except Exception as e:
            return f"Error invoking Claude: {str(e)}"

def main():
    parser = argparse.ArgumentParser(description='Claude Code Assistant')
    parser.add_argument('action', choices=['review', 'document', 'explain'])
    parser.add_argument('file_path', help='Path to code file')
    parser.add_argument('--context', help='Additional context for review')
    parser.add_argument('--function', help='Specific function to explain')
    parser.add_argument('--region', default='us-west-2', help='AWS region')
    
    args = parser.parse_args()
    
    assistant = ClaudeCodeAssistant(region=args.region)
    
    if args.action == 'review':
        result = assistant.review_code(args.file_path, args.context or "")
    elif args.action == 'document':
        result = assistant.generate_documentation(args.file_path)
    elif args.action == 'explain':
        result = assistant.explain_code(args.file_path, args.function)
    
    print(result)

if __name__ == '__main__':
    main()
```

### VS Code Integration for Claude

Create a VS Code extension configuration:

```json
{
  "contributes": {
    "commands": [
      {
        "command": "claude.reviewCode",
        "title": "Review with Claude",
        "category": "Claude Assistant"
      },
      {
        "command": "claude.generateDocs",
        "title": "Generate Documentation",
        "category": "Claude Assistant"
      },
      {
        "command": "claude.explainCode",
        "title": "Explain Code",
        "category": "Claude Assistant"
      }
    ],
    "menus": {
      "editor/context": [
        {
          "command": "claude.reviewCode",
          "group": "claude",
          "when": "editorHasSelection"
        },
        {
          "command": "claude.generateDocs",
          "group": "claude"
        },
        {
          "command": "claude.explainCode",
          "group": "claude"
        }
      ]
    }
  }
}
```

## CodeGuru Integration

### Automated Code Review Setup

```bash
#!/bin/bash
# Install CodeGuru CLI
pip3 install amazon-codeguru-cli

# Configure CodeGuru for automatic reviews
cat > ~/.codeguru/config.yaml << EOF
default:
  region: ${aws_region}
  auto_review: true
  review_on_commit: true
  exclude_patterns:
    - "*.min.js"
    - "node_modules/**"
    - "dist/**"
    - "build/**"
EOF

# Set up Git hooks for automatic review
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash
echo "Running CodeGuru review..."
codeguru-cli review --source-path .
EOF

chmod +x .git/hooks/pre-commit
```

## Intelligent Development Workflows

### Automated Code Quality Pipeline

Create `scripts/ai_quality_pipeline.sh`:

```bash
#!/bin/bash
set -euo pipefail

echo "Starting AI-powered code quality pipeline 🤖"

# 1. Security scanning with Q Developer
echo "Step 1: Security scanning..."
q security-scan --path . --format json > security_report.json

# 2. Code review with Claude
echo "Step 2: AI code review..."
find . -name "*.py" -o -name "*.js" -o -name "*.ts" -o -name "*.go" | while read file; do
    echo "Reviewing $file..."
    python3 /usr/local/bin/claude_assistant.py review "$file" > "reviews/$(basename $file).review.md"
done

# 3. Performance analysis with CodeGuru
echo "Step 3: Performance analysis..."
codeguru-cli performance-analysis --source-path .

# 4. Generate summary report
echo "Step 4: Generating summary..."
python3 /usr/local/bin/generate_quality_report.py

echo "Quality pipeline complete! Check the reports/ directory for results."
```

### AI-Powered Development Assistant

Create a comprehensive development assistant:

```python
#!/usr/bin/env python3
import os
import subprocess
import json
from pathlib import Path

class AIDevAssistant:
    def __init__(self):
        self.claude = ClaudeCodeAssistant()
        self.project_root = Path.cwd()
    
    def analyze_project(self):
        """Analyze entire project structure and provide insights"""
        analysis = {
            "structure": self._analyze_structure(),
            "dependencies": self._analyze_dependencies(),
            "code_quality": self._analyze_code_quality(),
            "security": self._analyze_security(),
            "recommendations": []
        }
        
        # Generate AI recommendations
        recommendations = self._generate_recommendations(analysis)
        analysis["recommendations"] = recommendations
        
        return analysis
    
    def _analyze_structure(self):
        """Analyze project structure"""
        structure = {}
        for root, dirs, files in os.walk(self.project_root):
            # Skip hidden and build directories
            dirs[:] = [d for d in dirs if not d.startswith('.') and d not in ['node_modules', 'dist', 'build']]
            
            rel_root = os.path.relpath(root, self.project_root)
            structure[rel_root] = {
                "directories": dirs,
                "files": files,
                "file_types": self._categorize_files(files)
            }
        
        return structure
    
    def _categorize_files(self, files):
        """Categorize files by type"""
        categories = {
            "source": [],
            "config": [],
            "documentation": [],
            "tests": [],
            "other": []
        }
        
        for file in files:
            ext = Path(file).suffix.lower()
            if ext in ['.py', '.js', '.ts', '.go', '.java', '.cpp', '.c', '.rs']:
                categories["source"].append(file)
            elif ext in ['.json', '.yaml', '.yml', '.toml', '.ini', '.cfg']:
                categories["config"].append(file)
            elif ext in ['.md', '.rst', '.txt', '.doc', '.docx']:
                categories["documentation"].append(file)
            elif 'test' in file.lower() or ext in ['.test.js', '.spec.js']:
                categories["tests"].append(file)
            else:
                categories["other"].append(file)
        
        return categories
    
    def setup_ai_workflows(self):
        """Set up AI-powered development workflows"""
        workflows = {
            "pre_commit": self._setup_pre_commit_ai(),
            "ci_cd": self._setup_ci_cd_ai(),
            "code_review": self._setup_code_review_ai(),
            "documentation": self._setup_doc_generation()
        }
        
        return workflows

# Usage in template startup script
if __name__ == "__main__":
    assistant = AIDevAssistant()
    analysis = assistant.analyze_project()
    workflows = assistant.setup_ai_workflows()
    
    print("AI Development Assistant initialized!")
    print(f"Project analysis complete: {len(analysis['structure'])} directories analyzed")
    print(f"AI workflows configured: {', '.join(workflows.keys())}")
```

## Template Integration Example

Here's how to integrate all AI services into a complete template:

```hcl
# AI-Enhanced Development Template
resource "aws_instance" "ai_workspace" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  
  # Enhanced IAM role with AI service permissions
  iam_instance_profile = aws_iam_instance_profile.ai_enhanced_profile.name
  
  vpc_security_group_ids = [aws_security_group.ai_workspace.id]
  subnet_id              = var.subnet_id
  
  user_data = templatefile("${path.module}/ai_startup.sh", {
    username   = data.coder_workspace_owner.me.name
    aws_region = var.aws_region
  })
  
  tags = {
    Name     = "coder-ai-workspace-${data.coder_workspace_owner.me.name}"
    Owner    = data.coder_workspace_owner.me.name
    Features = "ai-enhanced"
    Coder    = "true"
  }
}

# AI Assistant Dashboard
resource "coder_app" "ai_dashboard" {
  agent_id     = coder_agent.ai_enhanced.id
  slug         = "ai-dashboard"
  display_name = "AI Assistant Dashboard"
  icon         = "/icon/ai.svg"
  url          = "http://localhost:3000/ai-dashboard"
  subdomain    = true
  share        = "owner"
}
```

{{% notice tip %}}
🤖 **AI Best Practices**: Start with basic AI integration and gradually add more sophisticated features. Monitor usage patterns to optimize AI service costs.
{{% /notice %}}

{{% notice info %}}
🔒 **Privacy & Security**: Ensure sensitive code isn't sent to AI services without proper review. Configure data retention policies appropriately.
{{% /notice %}}

### Next Steps
Now let's explore advanced template features including networking, persistent storage, and enterprise security policies.