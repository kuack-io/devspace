# Devspace

Development environment configuration for kuack using DevSpace and k3d.

## Prerequisites

- [Docker](https://docs.docker.com/engine/install/) - Container platform
- [k3d](https://k3d.io/stable/#installation) - Lightweight wrapper to run k3s clusters in Docker
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) - Kubernetes command-line tool
- [DevSpace](https://www.devspace.sh/docs/getting-started/installation) - Development tool for Kubernetes

## Getting Started

### Create k3d Cluster

Create a k3d cluster with port forwarding for ingress

```bash
k3d cluster create kuack --agents 2 --port "8077:80@loadbalancer"
```

**Note:** docker commands may require `sudo` depending on your system configuration. If that's the case, then use `sudo` for `k3d` commands as well.

**Important:** The `--port "8077:80@loadbalancer"` flag maps port 8077 on your host to port 80 on the k3d loadbalancer, allowing you to access ingress resources at `http://localhost:8077` (or `https://localhost:8077` if TLS is configured).

### Switch Context

Switch `kubectl` context to the new cluster:

```bash
kubectl config use-context k3d-kuack
```

Create new namespace and switch to it:

```bash
kubectl create namespace kuack
kubectl config set-context --current --namespace=kuack
```

### Deploy with DevSpace

```bash
devspace deploy
```

### Access the Application

After deployment, access the application via <http://kuack-agent.localhost:8077>.
Use <http://kuack-node.localhost:8077> as a target.

### Development Mode

**Note:** This configuration assumes that the `node/` and `agent/` folders with the source code are located at the same level as the `devspace/` directory in your workspace.

Start development mode with hot-reload:

```bash
devspace dev
```

Be patient, it make take some time for the apps to download dependencies and recompile.
