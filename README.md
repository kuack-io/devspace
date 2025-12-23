# Devspace

Development environment configuration for kuack using DevSpace and k3d.

## Prerequisites

- [k3d](https://k3d.io/) - Lightweight wrapper to run k3s in Docker
- [DevSpace](https://www.devspace.sh/) - Development tool for Kubernetes
- Docker (or other runtime)

## Getting Started

### Create k3d Cluster

Create a k3d cluster with a local registry and port forwarding for ingress (use `sudo` if your container )

```bash
k3d cluster create kuack \
  -p "8077:80@loadbalancer" \
  --agents 2
```

Merge/import cluster config:

```bash
k3d kubeconfig merge kuack --output ~/.kube/config
```

or, if that's your first cluster:

```bash
mkdir -p ~/.kube/
k3d kubeconfig get kuack > ~/.kube/config
```

**Important:** The `-p "8077:80@loadbalancer"` flag maps port 8077 on your host to port 80 on the k3d loadbalancer, allowing you to access ingress resources at `http://localhost:8077` (or `https://localhost:8077` if TLS is configured).

### Deploy with DevSpace

```bash
devspace deploy
```

### Access the Application

After deployment, access the application via <http://kuack-agent.localhost:8077>.
Use <ws://kuack-node.localhost:8077> as a target.

### Development Mode

**Note:** This configuration assumes that the `node/` and `agent/` folders with the source code are located at the same level as the `devspace/` directory in your workspace.

Start development mode with hot-reload:

```bash
devspace dev
```
