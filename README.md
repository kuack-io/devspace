# Devspace

Development environment configuration for kuack using DevSpace on top of Minikube.

## Prerequisites

- [Docker](https://docs.docker.com/engine/install/) - Container runtime used by Minikube
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) - Kubernetes command-line tool
- [Minikube](https://minikube.sigs.k8s.io/docs/start/) - Local Kubernetes cluster (v1.34+ recommended)
- [DevSpace](https://www.devspace.sh/docs/getting-started/installation) - Development tool for Kubernetes

## Getting Started

### Start Minikube

```bash
minikube start --cpus=4 --memory=8g --disk-size=40g
```

### Configure kubectl context and namespace

```bash
kubectl config use-context minikube
kubectl create namespace kuack
kubectl config set-context --current --namespace=kuack
```

### Deploy with DevSpace

```bash
devspace deploy
```

### Development Mode

**Note:** This configuration assumes that the `node/` and `agent/` folders with the source code are located at the same level as the `devspace/` directory in your workspace.

Start development mode with hot reload:

```bash
devspace dev
```

Be patient, it make take some time for the apps to download dependencies and recompile.

Dev mode automatically performs port forwarding.

### Access the Application

- Agent UI: <http://localhost:8080>
- Node endpoint: <http://localhost:8081>

If you only ran `devspace deploy`, forward the services manually:

```bash
kubectl port-forward svc/kuack-agent 8080:8080
kubectl port-forward svc/kuack-node 8081:8080
```
