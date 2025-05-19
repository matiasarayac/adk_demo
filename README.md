# ADK Simple Agent: Creation, Local Testing, and Deployment Guide

This project demonstrates how to create a simple agent using the Agent Development Kit (ADK), test it locally, and deploy it to an Agent Engine.

## Table of Contents

- [Project Setup](#project-setup)
- [Running the agent](#running-the-agent)
- [Deployment on Vertex AI Agent Engine](#deployment-on-vertex-ai-agent-engine)

## Project Setup

Follow these steps to get your development environment set up and running using Poetry.

### 1. Prerequisites
Before you begin, ensure you have the following installed:
- **Python**: `v3.10` or later.
- **Git**: For cloning the repository.
- **Poetry**: For dependency management. If you don't have Poetry installed, you can install it by following the official instructions: Poetry Installation Guide

### 2. Clone the Repository
```bash
git clone <your-repository-url>
cd <your-project-directory>
```

### 3. Install Dependencies with Poetry:

```bash
poetry install
```

### 4. Activate the Poetry Shell:
```bash
poetry env activate
```

### 5. Set up Environment Variables:
Copy the example environment file:
```bash
cp .env.example .env
```
Then, update the `.env` file with your specific configurations.

```bash
# Choose Model Backend: 0 -> ML Dev, 1 -> Vertex
GOOGLE_GENAI_USE_VERTEXAI=1

# ML Dev backend config. Fill if using Ml Dev backend.
GOOGLE_API_KEY='YOUR_VALUE_HERE'

# Vertex backend config
GOOGLE_CLOUD_PROJECT='YOUR_VALUE_HERE'
GOOGLE_CLOUD_LOCATION='YOUR_VALUE_HERE'
```

## Running the Agent

You can run the agent using the ADK command in your terminal. from the working directory:

1. Run agent using the CLI:
```bash
cd src/
poetry run adk run adk_demo
```

2. Run agent with ADK Web UI:
```bash
poetry run adk web
```

## Deployment on Vertex AI Agent Engine

```bash
poetry build --format=wheel --output=deployment
```

This will create a file named adk_demo-0.1.0-py3-none-any.whl in the deployment directory.

Then run the below command. This will create a staging bucket in your GCP project and deploy the agent to Vertex AI Agent Engine:
```bash
cd deployment/
poetry run python3 deploy.py --create
```

When this command returns, if it succeeds it will print an AgentEngine resource name that looks something like this:
```bash
projects/************/locations/us-central1/reasoningEngines/7737333693403889664
```

Once you have successfully deployed your agent, you can interact with it using the test_deployment.py script in the deployment directory. Store the agent's resource ID in an environment variable and run the following command:

```bash
export RESOURCE_ID=...
export USER_ID=<any string>
poetry run python3 test_deployment.py --resource_id=$RESOURCE_ID --user_id=$USER_ID
```

To delete the agent, run the following command (using the resource ID returned previously):
```bash
poetry run python3 deployment/deploy.py --delete --resource_id=$RESOURCE_ID
```