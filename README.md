# 🚀 AgentOrbit – Cloud-Native AI Agent Application on Microsoft Azure

**AgentOrbit** is a React-based Generative AI application integrated with Microsoft Foundry and deployed on Azure Kubernetes Service (AKS).

The project demonstrates how to build, containerize and deploy an AI-powered application using Microsoft Azure cloud services and Kubernetes.

It also explores a production-oriented architecture with a dedicated backend API, secure authentication, infrastructure automation, monitoring and scalable AI services.

---

## 🧱 1. Current Architecture – Implemented POC

The current implementation uses React as the frontend, Microsoft Foundry for AI capabilities, Azure Container Registry (ACR) for container image management and Azure Kubernetes Service (AKS) for application hosting.

![AgentOrbit Current Architecture](https://github.com/user-attachments/assets/b180970b-bdf8-4c7f-959d-f114b384861d)

### Architecture Components

| Component | Responsibility |
|---|---|
| React UI | User interface and conversational experience |
| Microsoft Foundry | AI model / agent endpoint |
| Azure Container Registry | Private container image storage |
| Azure Kubernetes Service | Container orchestration and application hosting |
| Kubernetes | Application deployment and workload management |

### Current Implementation Flow

**AI Request Flow**

```text
User
 |
 v
React UI (Running on AKS)
 |
 | HTTPS Request
 v
Microsoft Foundry
 |
 v
AI Model / Agent
 |
 v
AI Response
 |
 v
React UI
```

**Application Deployment Flow**

```text
React Source Code
       |
       v
Azure ACR Tasks
       |
       v
Azure Container Registry
       |
       | Container Image Pull
       v
Azure Kubernetes Service
       |
       v
React Application Running
```

**Note:** ACR and AKS are part of the application deployment and hosting flow. ACR stores the container image, while AKS pulls and runs it. The current React application communicates directly with Microsoft Foundry.

---

## 🚀 2. Proposed Architecture – Production Evolution

The next architectural iteration introduces a dedicated backend API layer to improve security, maintainability, scalability and separation of concerns.

![AgentOrbit Proposed Architecture](https://github.com/user-attachments/assets/1e6025fb-0352-4dc3-b145-ca95761e00d5)

### Proposed Architecture Components

| Component | Responsibility |
|---|---|
| React | Frontend user interface |
| .NET 8 Web API / Azure Functions | Backend API, validation and AI orchestration |
| Microsoft Entra ID | Authentication and authorization |
| Microsoft Foundry | AI model and agent execution |
| Azure Key Vault | Secrets and configuration management |
| Managed Identity | Secure service-to-service authentication |
| Azure Monitor | Monitoring, logs and observability |
| Azure Container Registry | Container image management |
| Azure Kubernetes Service | Container orchestration and workload scaling |

### Planned Request Flow

```text
                 User
                  |
                  v
              React UI
                  |
                  | HTTPS
                  v
           .NET 8 Web API
                  |
          Authentication
          Request Validation
          Business Logic
                  |
                  v
          Microsoft Foundry
                  |
                  v
            AI Model / Agent
                  |
                  v
             API Response
                  |
                  v
               React UI
```

### Supporting Azure Services

```text
Microsoft Entra ID  → Authentication
Azure Key Vault     → Secrets Management
Managed Identity    → Service Authentication
Azure Monitor       → Observability
Azure ACR           → Container Images
Azure AKS           → Application Hosting
```

**Architecture goal:** Separate frontend, backend and AI responsibilities while keeping the system secure, scalable and independently deployable.

This is a proposed target architecture, not the current implementation.

---

## 🎬 3. Application Demo

Watch AgentOrbit in action.

[▶️ View AgentOrbit Application Demo](https://github.com/user-attachments/assets/7317c6ef-c6ba-4996-81aa-70d163f66c28)

The demo showcases the React chat interface and its integration with Microsoft Foundry.

---

## ⚙️ 4. Features

### Implemented

- Interactive AI chat interface using React.
- Microsoft Foundry model integration.
- Containerized React application.
- Remote container image build using Azure ACR Tasks.
- Container image storage in Azure Container Registry.
- AKS integration with ACR.
- Kubernetes-based application deployment.
- Application testing using kubectl port forwarding.

### Planned

- Dedicated backend API layer.
- Secure authentication and authorization.
- Advanced AI agent orchestration and tool calling.
- Conversation history and memory.
- Retrieval-Augmented Generation (RAG).
- Centralized logging and monitoring.
- Automated CI/CD pipelines.

---

## 🛠️ 5. Technology Stack

| Category | Technology |
|---|---|
| Frontend | React.js |
| Generative AI | Microsoft Foundry |
| Containerization | Docker |
| Container Registry | Azure Container Registry |
| Orchestration | Azure Kubernetes Service |
| Deployment | Kubernetes YAML |
| Cloud Platform | Microsoft Azure |
| CLI | Azure CLI |
| Kubernetes Management | kubectl |
| Planned Backend | .NET 8 / Azure Functions |
| Planned Infrastructure as Code | Terraform |
| Planned CI/CD | GitHub Actions |

---

## ▶️ 6. Setup and Deployment Guide

### Prerequisites

- Microsoft Azure subscription
- Node.js and npm
- Azure CLI
- kubectl
- Git
- Azure Container Registry
- Azure Kubernetes Service
- Microsoft Foundry project and deployed model

### Step 1: Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/agent-orbit.git

cd agent-orbit
```

### Step 2: Configure Microsoft Foundry

1. Create a Microsoft Foundry resource.
2. Create a Foundry project.
3. Deploy a supported AI model or configure an agent.
4. Obtain the endpoint and authentication configuration.
5. Configure the React application to communicate with Foundry.

Keep credentials out of source control. The current direct frontend API-key approach is suitable only for demonstration.

### Step 3: Install and Run React

```bash
npm install

npm run dev
```

Verify the application locally before building the container image.

### Step 4: Login to Azure

```powershell
az login

az account set --subscription "YOUR_SUBSCRIPTION_ID"

az account show -o table
```

### Step 5: Build and Push Container Image to ACR

This project uses Azure Container Registry Tasks to build the image remotely, avoiding the need for a local Docker Desktop installation.

```powershell
az acr build `
  --registry YOUR_ACR_NAME `
  --image agent-orbit:v1 .
```

Verify the image:

```powershell
az acr repository list `
  --name YOUR_ACR_NAME `
  -o table
```

### Step 6: Connect AKS with ACR

Grant AKS permission to pull images from the registry:

```powershell
az aks update `
  --resource-group YOUR_RESOURCE_GROUP `
  --name YOUR_AKS_CLUSTER `
  --attach-acr YOUR_ACR_NAME
```

### Step 7: Connect to AKS

```powershell
az aks get-credentials `
  --resource-group YOUR_RESOURCE_GROUP `
  --name YOUR_AKS_CLUSTER
```

Verify cluster connectivity:

```powershell
kubectl get nodes

kubectl get pods -A
```

### Step 8: Configure Kubernetes Deployment

Update `manifests/deployment.yaml` with the correct ACR image reference.

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: agent-orbit
spec:
  replicas: 1
  selector:
    matchLabels:
      app: agent-orbit
  template:
    metadata:
      labels:
        app: agent-orbit
    spec:
      containers:
        - name: agent-orbit
          image: YOUR_ACR_NAME.azurecr.io/agent-orbit:v1
          ports:
            - containerPort: 80
```

### Step 9: Deploy Application

```powershell
kubectl apply -f manifests/deployment.yaml

kubectl get deployments

kubectl get pods

kubectl describe deployment agent-orbit
```

### Step 10: Access Application

For local testing:

```powershell
kubectl port-forward deployment/agent-orbit 8080:80
```

Open the application:

```text
http://localhost:8080
```

---

## ☁️ 7. Azure Infrastructure Details

The following resources were used in the learning environment.

| Resource | Configuration |
|---|---|
| Cloud Provider | Microsoft Azure |
| Region | Sweden Central |
| Resource Group | rg-ai-agents-aks-dev |
| AKS Cluster | aks-ai-agents-dev |
| Azure Container Registry | regkubaiagents |
| Container Repository | aoaichatapp (original image repository) |
| Image Tag | v1 |
| AI Platform | Microsoft Foundry |
| Frontend | React |
| Orchestration | Kubernetes |

**Note:** Update the container repository and image reference if you rename the original `aoaichatapp` image to `agent-orbit`.

---

## 🔐 8. Security Considerations

Security is an important part of evolving AgentOrbit towards production.

### Current POC

- React currently communicates directly with Microsoft Foundry.
- API-key authentication is configured in the frontend.
- This approach is not suitable for a public production deployment because frontend credentials can be inspected.

### Planned Security Improvements

- Move Foundry API communication into a secure backend.
- Integrate Microsoft Entra ID authentication.
- Use managed identities where supported.
- Store secrets in Azure Key Vault.
- Apply least-privilege RBAC permissions.
- Enforce HTTPS and secure ingress.
- Implement API validation and rate limiting.
- Avoid committing credentials and configuration secrets to GitHub.

---

## 🏗️ 9. Production Readiness & Engineering Roadmap

The following improvements are planned to evolve the current POC into a more production-ready cloud-native application.

### 1. Backend API Layer

- Introduce a .NET 8 Web API or Azure Functions.
- Move AI communication from React to the backend.
- Implement request validation and centralized exception handling.
- Add API versioning and rate limiting.
- Implement retry policies and timeout handling.
- Support streaming AI responses.

### 2. AI Agent Capabilities

- Implement agent orchestration and tool calling.
- Add Retrieval-Augmented Generation (RAG).
- Integrate Azure AI Search for knowledge retrieval.
- Add conversation history and context management.
- Implement responsible AI controls and content filtering.

### 3. Identity and Security

- Microsoft Entra ID authentication.
- Managed Identity for Azure service access.
- Azure Key Vault integration.
- Role-based access control.
- Secure ingress and network policies.

### 4. Kubernetes and Infrastructure

- Configure Horizontal Pod Autoscaler (HPA).
- Add readiness, liveness and startup probes.
- Define CPU and memory requests and limits.
- Configure ingress and TLS.
- Implement network policies.
- Use Terraform for repeatable infrastructure provisioning.

### 5. CI/CD and DevOps

- GitHub Actions for automated build and deployment.
- Automated container image publishing to ACR.
- Versioned image releases.
- Container vulnerability scanning.
- Automated deployment and rollback workflows.
- Environment-specific configuration.

### 6. Monitoring and Reliability

- Azure Monitor integration.
- Container Insights.
- Centralized application logging.
- Distributed tracing.
- AI endpoint latency and error monitoring.
- Operational alerts and dashboards.
- Backup and recovery planning.

---

## 📌 10. Key Engineering Highlights

- Integrated Generative AI using Microsoft Foundry.
- Developed a React-based conversational AI interface.
- Built container images remotely using Azure ACR Tasks.
- Used Azure Container Registry for image management.
- Deployed the application on Azure Kubernetes Service.
- Used Kubernetes manifests for declarative deployment.
- Demonstrated separation of container deployment and AI inference flows.
- Designed a target architecture for secure backend integration and production evolution.

---

## 📊 11. Project Status

| Capability | Status |
|---|---|
| React AI Interface | Implemented |
| Microsoft Foundry Integration | Implemented |
| ACR Image Build | Implemented |
| AKS Deployment | Implemented |
| Kubernetes Manifest | Implemented |
| Secure Backend API | Planned |
| Entra ID Authentication | Planned |
| Managed Identity | Planned |
| Key Vault | Planned |
| Terraform | Planned |
| GitHub Actions CI/CD | Planned |
| Monitoring and Observability | Planned |

**Project classification:** Production-oriented AI application POC. The application is not yet fully production-hardened.

---

## 🚀 12. Future Vision

AgentOrbit aims to evolve into a cloud-native AI agent application with:

- Secure API-driven AI integration.
- Agent orchestration and external tool integration.
- Scalable containerized workloads.
- Automated infrastructure provisioning.
- Continuous integration and deployment.
- Secure identity and secrets management.
- End-to-end observability and operational monitoring.

The objective is to explore the engineering practices required to take a Generative AI application from a working prototype towards a secure, scalable and maintainable cloud deployment.

---

## 👨‍💻 Author

Built as part of a hands-on Cloud Engineering and Generative AI learning journey.

**Project:** AgentOrbit

**Focus Areas:** Microsoft Azure | Generative AI | Microsoft Foundry | React | Kubernetes | Cloud-Native Architecture | DevOps

