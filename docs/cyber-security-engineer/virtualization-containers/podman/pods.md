# Podman — Pods

A **pod** groups one or more containers together so they can be managed as a single unit — the same concept Kubernetes uses for its pods. Podman can create and run pods locally, including **rootless** pods.

## Creating a rootless pod

Create a pod:

```bash
podman pod create
```

Create a pod and publish it through a single port — this maps host port 8081 to port 80 inside the pod, and names the pod `p3-web`:

```bash
podman pod create --name p3-web -p 8081:80
```

Add a container to that pod:

```bash
podman run --pod p3-web -d --name p3-web docker.io/library/nginx:latest
```

## YAML file basics

Podman pods can also be described declaratively in a YAML manifest, similar in spirit to a Kubernetes pod spec.

`apiVersion` identifies the API schema version being used:

```yaml
apiVersion: v1
```

`kind` declares what kind of object this manifest describes — here, a pod:

```yaml
kind: Pod
```

`metadata` (with a `name` field) gives the pod its identity:

```yaml
metadata:
  name: <pod-name>
```

`spec` describes what should actually run inside the pod, starting with its `containers`:

```yaml
spec:
  containers:
```

Each container entry needs a `name`, and an `image` to pull (here, from Docker Hub):

```yaml
- name: <container name>
  image: nginx:latest
```

`ports` exposes a container port on the host — here, nginx listens on port 80 inside the pod, and that's exposed on host port 8080:

```yaml
ports:
  - containerPort: 80 # nginx listens on 80 inside the pod
    hostPort: 8080     # expose it on the host at 8080
```
