# K8s-Go-Microservices-r ☸️🚀

> Production-grade cloud-native microservices architecture in Go (Golang) and gRPC, deployed and orchestrated on Kubernetes with Tilt, Skaffold, and Google Cloud (GKE).

## 📌 Overview
**K8s-Go-Microservices-r** is an enterprise-ready distributed microservices platform modeled around an event-driven ride-sharing system. It implements modern cloud-native architectural patterns:
- **High-Performance RPC Communication:** Inter-service communication via Protocol Buffers (`proto/`) and gRPC.
- **API Gateway Pattern:** Unified entrypoint routing HTTP and gRPC traffic.
- **Local-to-Cloud Kubernetes Orchestration:** Fast live-reloading container environments using [Tilt](https://tilt.dev/) and Minikube, paired with production Google Kubernetes Engine (GKE) deployments.
- **Next.js Web Client:** Modern responsive dashboard interface under `/web`.

## 🛠️ Architecture & Tech Stack
- **Backend Languages:** Go (Golang 1.22+)
- **API Protocols:** gRPC, Protocol Buffers (Protobuf), REST HTTP
- **Container Orchestration:** Kubernetes (K8s), Minikube, Google Kubernetes Engine (GKE)
- **Local Dev Tools:** Tilt (`Tiltfile`), Skaffold (`skaffold.yaml`), Make
- **CI/CD & Cloud:** Google Cloud Build (`cloudbuild.yaml`), GCP Artifact Registry
- **Frontend:** Next.js (Node.js 20+)

## 📁 Repository Structure
```text
├── proto/                    # Protocol Buffer definitions for inter-service contracts
├── services/                 # Microservices
│   ├── api-gateway/          # Unified ingress API gateway (routing, auth, proxy)
│   └── driver-service/       # Core driver management & dispatch logic
├── shared/                   # Common Go utilities, logging, and error handling
├── infra/                    # Kubernetes manifests & production Dockerfiles
│   ├── local/                # Minikube / Tilt deployment manifests
│   └── production/           # GKE production deployment configs & Dockerfiles
├── web/                      # Next.js web application frontend
├── Tiltfile                  # Tilt live container orchestration config
├── skaffold.yaml             # Skaffold workflow specification
├── cloudbuild.yaml           # Google Cloud Build CI/CD pipeline
├── Makefile                  # Build and code-gen automation
└── README.md
```

## ⚙️ Quick Start

### 1. Prerequisites
- [Go 1.22+](https://go.dev/)
- [Docker](https://www.docker.com/)
- [Minikube](https://minikube.sigs.k8s.io/) or kind
- [Tilt](https://tilt.dev/)
- [Node.js 20+](https://nodejs.org/) (for web client)

### 2. Local Kubernetes Cluster
Start your local Kubernetes cluster:
```bash
minikube start
```

### 3. Launch with Tilt (Live Reloading)
```bash
tilt up
```
Open the Tilt UI in your browser to monitor pod build cycles, live reload triggers, and stream aggregate logs.

### 4. Verify Pods & Services
```bash
kubectl get pods
kubectl get services
```

## ☁️ Production Deployment (GCP / GKE)
Build and push images to Google Artifact Registry:
```bash
# Set your GCP variables
export REGION=us-central1
export PROJECT_ID=your-project-id

# Build & Push API Gateway
docker build -t ${REGION}-docker.pkg.dev/${PROJECT_ID}/ride-sharing/api-gateway:latest \
  --platform linux/amd64 -f infra/production/docker/api-gateway.Dockerfile .
docker push ${REGION}-docker.pkg.dev/${PROJECT_ID}/ride-sharing/api-gateway:latest

# Build & Push Driver Service
docker build -t ${REGION}-docker.pkg.dev/${PROJECT_ID}/ride-sharing/driver-service:latest \
  --platform linux/amd64 -f infra/production/docker/driver-service.Dockerfile .
docker push ${REGION}-docker.pkg.dev/${PROJECT_ID}/ride-sharing/driver-service:latest
```

Apply Kubernetes production manifests:
```bash
kubectl apply -f infra/production/k8s/
```

## 📄 License
MIT License
