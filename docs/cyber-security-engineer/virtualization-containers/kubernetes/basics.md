# Kubernetes — Basics

**minikube** is a tool that creates a Kubernetes cluster locally for testing purposes — a quick way to get a working cluster without needing real cluster infrastructure.

**kubectl** is the command-line tool used to manage that cluster (local ones like minikube, or a real one).

Start minikube:

```bash
minikube start
```

Create a deployment from an image:

```bash
kubectl create deployment --image="image" "name"
```

List all deployments:

```bash
kubectl get deployments
```

List all pods:

```bash
kubectl get pods
```

List all services:

```bash
kubectl get services
```

List all ReplicaSets:

```bash
kubectl get replicaset
```

List everything at once:

```bash
kubectl get all
```

Apply a manifest file to the cluster:

```bash
kubectl apply -f pod.yaml
```

Show detailed information about a specific pod:

```bash
kubectl describe pod "pod_name"
```
