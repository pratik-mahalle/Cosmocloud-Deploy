
# 🌌 Cosmocloud-Deploy Helm Chart 🚀

This Helm chart is designed to deploy a complete Cosmocloud application stack consisting of a **Backend**, **Frontend**, and **Redis** database using Kubernetes. 

## 📜 Table of Contents
- [✨ Introduction](#-introduction)
- [⚙️ Installation](#-installation)
- [🔗 Services](#-services)
- [⚡ Configuration](#-configuration)
- [🔍 Testing the Application](#-testing-the-application)
- [🗑️ Uninstallation](#️-uninstallation)

## ✨ Introduction

The Helm chart deploys the following components:

- **Backend**: A backend service that connects to Redis for data storage and exposes an API.
- **Frontend**: A frontend service that communicates with the backend and presents the user interface.
- **Redis**: A Redis service used by the backend for data storage.

Each service is exposed using different service types:

- **Backend**: Exposed via `ClusterIP` (internal communication only).
- **Frontend**: Exposed via `NodePort` to allow external access.
- **Redis**: Exposed via `ClusterIP` (internal communication only).

## ⚙️ Installation

Follow the steps below to deploy the Helm chart:

### Prerequisites 🛠️
Ensure that you have the following installed:
- [Helm](https://helm.sh/docs/intro/install/) (version 3+)
- [Kubernetes](https://kubernetes.io/docs/setup/) cluster running (e.g., Minikube, kind, GKE, AKS, EKS)


### Steps to Install

1. **📂 Clone the Repository**:
   Clone the repository containing the Helm chart to your local machine:
   ```bash
   git clone <repository-url>
   cd cosmocloud-deploy
   ```

2. **📦 Install the Helm Chart**:
   Install the Helm chart by running the following command:
   ```bash
   helm install testapp cosmocloud-deploy --atomic --timeout 30s
   ```

   - `testapp`: The release name for the Helm deployment.
   - `cosmocloud-deploy`: The path to the Helm chart directory.

3. **✅ Verify the Deployment**:
   Ensure that the components are successfully deployed by running:
   ```bash
   kubectl get deployments 
   kubectl get services 
   ```

   You should see the following resources:
   - **Backend**: 1 replica, `ClusterIP` service on port 8000.
   - **Frontend**: 1 replica, `NodePort` service on port 5175 (NodePort 31000).
   - **Redis**: 1 replica, `ClusterIP` service on port 6379.

## 🔗 Services

- **Backend Service**: 
  - **Type**: `ClusterIP`
  - **Port**: 8000
  - This service exposes the backend application internally within the Kubernetes cluster.
  
- **Frontend Service**: 
  - **Type**: `NodePort`
  - **Port**: 5175
  - **NodePort**: 31000
  - This service exposes the frontend application externally.

- **Redis Service**: 
  - **Type**: `ClusterIP`
  - **Port**: 6379
  - This service exposes Redis internally to the backend.

## ⚡ Configuration

The backend and frontend services are configured using Kubernetes `ConfigMap` resources. These configurations pass environment variables to the applications.

### 🌐 Environment Variables

- **Backend**:
  - **REDIS_URI**: `redis://redis-svc:6379` – The URI for connecting to the Redis service.

- **Frontend**:
  - **BACKEND_URL**: `http://backend-svc:8000` – The URL for connecting to the backend service.

The environment variables are set in `ConfigMap` resources (`backend-config` and `frontend-config`) and are referenced by the deployments.


## 🔍 Testing the Application

Once the Helm chart is deployed, you can test the application:

1. **🌐 Access the Frontend**:
   Since the frontend service is exposed via `NodePort`, you can access it externally. First, find your node's IP:
   
   For **Minikube** users:
   ```bash
   minikube ip
   ```

   Then, open a browser and navigate to:
   ```
   http://<node-ip>:31000
   ```

2. **📄 Verify Backend Logs**:
   Check if the backend is correctly connecting to Redis by inspecting the logs:
   ```bash
   kubectl logs <backend-pod-name> -n default
   ```

3. **📄 Verify Frontend Logs**:
   Ensure the frontend is communicating with the backend:
   ```bash
   kubectl logs <frontend-pod-name> -n default
   ```

## 🗑️ Uninstallation

To remove the application and all resources from the cluster, run:
```bash
helm uninstall testapp
```

This will delete the backend, frontend, Redis, and associated resources.

(Fun Fact: This was my assignment for the role of devops intern, so I thaught it might help someone😀)
