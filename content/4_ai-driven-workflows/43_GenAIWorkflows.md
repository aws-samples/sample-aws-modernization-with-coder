---
title: "GenAI RAG Application Development"
chapter: false
weight: 53
---

## Building Intelligent Applications with Retrieval-Augmented Generation

GenAI RAG (Retrieval-Augmented Generation) workflows represent the cutting edge of AI application development, combining the power of large language models with domain-specific knowledge retrieval. This approach enables developers to build intelligent applications that can reason over custom datasets while maintaining accuracy and reducing hallucinations.

### Workflow 3: RAG Application Prototyping with Claude Code
#### Scenario: Build an intelligent document Q&A system using AWS Bedrock and Aurora pgvector

Let's create a complete RAG application development environment that combines Kubernetes scalability, Aurora PostgreSQL with vector search, and Claude Code for intelligent development assistance.

#### Step 1: Create Your RAG Development Workspace

Create a workspace using the AWS Workshop - Kubernetes with Claude Code RAG template:
1. **Access your Coder dashboard** and click "Create Workspace"
2. **Select the AWS Workshop - Kubernetes with Claude Code RAG template** from the dropdown
3. **Configure the workspace parameters**:
   - **Name**: `rag-dev-workspace`
   - **CPU**: 4 Cores (recommended for RAG workloads)
   - **Memory**: 6 GB (recommended for vector processing)
   - **Home disk size**: 30 GB
   - **AI Prompt**: `Look for an AWS RAG Prototyping repo in the Coder Workspace. If found, create a new Python3 virtual environment, pip install the requirements.txt and then start the app via streamlit.`

4. **Click "Create Workspace"** and wait for it to start

::alert[This template automatically provisions a Kubernetes pod with Aurora PostgreSQL Serverless v2 (with pgvector extension), Claude Code integration, and all necessary development tools for RAG application development.]{type="info"}

#### Step 2: Explore the RAG Development Environment

Once your workspace is running:

1. **Open the workspace** from your Coder dashboard
2. **View task** to access Task UI and monitor Claude Code

#### Step 3: Validate RAG Application Environment

The Claude Code agent will automatically begin setting up your RAG development environment based on the AI prompt. Monitor its progress:

1. **Observe Claude Code activities** in the integrated Task UI
2. **Review the RAG application structure** in the coder-server tab:

```bash
# Typical RAG application structure
rag-prototype/
├── app.py                  # Streamlit main application
├── requirements.txt        # Python dependencies
├── src/
│   ├── embeddings/        # Document embedding logic
│   ├── retrieval/         # Vector search implementation
│   ├── generation/        # LLM response generation
│   └── database/          # Aurora pgvector integration
├── data/                  # Sample documents
└── config/                # Configuration files
```

#### Step 4: Test RAG Components within Streamlit App

Validate the current RAG functionality and PostgreSQL VectorDB integration using Task UI / Preview Tab :

1. **Document Ingestion:** by Browsing local PDF's and selecting, then Processing 
2. **Vector Search & RAG Response Implementation:** by using Chat interace to post a query about Documents ingested
3. **Multi-Model Selection:** by selecting a different model from the dropdown and re-running previous or new query

#### Step 5: Protype new funcationality with AI Guidance

Use Claude Code to enhance your RAG application:

```bash
# Prompt for adding a Web Crawler for Data Ingestion
"Analyze the current RAG implementation and add support for crawling web pages to ingest additional data into the PostgreSQL Vector Knowledgebase.  Limit the number of web pages that could be ingested to 100, and maximum link depth of 4.  Process the data in chunks and ensure appropriate error handling and basic import progress tracking is available in the UI."
```
1. **Monitor Claude Agent** and provide any guidance needed to implement Web Crawler
2. **Test Vector Search & RAG Response Implementation** by crawling some web content and validating chat interface against new content


#### Step 6: Production Readiness Assessment in Kiro IDE

Leverage the Agentic capabilities of the Kiro IDE to perform a Well-Architected Production Readiness review:

1. **In the Task UI** select Open locally, then Kiro IDE
2. **Complete Kiro Sign-In** using AWS Builder ID or preferred credentials
3. **Open a new Vibe Session** in the right-hand IDE panel
```bash
# Prompt for production assessment
"Review the RAG application for production deployment. Conduct a Well-Architected review, checking for security best practices, error handling, logging, monitoring, scalability considerations, and AWS service integration patterns. Provide a deployment checklist."
```

::alert[🧠 *RAG Development Best Practices*: Start with a simple document type and query pattern. Gradually add complexity as you understand your use case better. Monitor embedding quality and retrieval relevance to optimize the system iteratively.]{type="info"}

### Integration with AWS Services

The template provides seamless integration with:
- **AWS Bedrock**: For embeddings and LLM inference
- **Aurora PostgreSQL**: For vector storage and metadata
- **Amazon EKS**: For scalable deployment

::alert[🚀 *Production Scaling*: This RAG development environment can be extended to production-scale deployments using Amazon EKS, Aurora Global Database, and Amazon Bedrock's provisioned throughput for consistent performance.]{type="info"}

## Next Steps

You've now experienced the complete spectrum of AI-driven development workflows:
1. **AI-Assisted Development** - Intelligent code generation and assistance
2. **AI-Driven Automation** - Autonomous task execution and workflow management  
3. **GenAI RAG Applications** - Building intelligent applications with domain knowledge

These workflows demonstrate how AI transforms software development from reactive coding to proactive, intelligent system building. The combination of Coder's development environments with AWS AI services creates a powerful platform for next-generation application development.