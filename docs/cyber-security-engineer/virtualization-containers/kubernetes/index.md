# Kubernetes — Overview

Kubernetes, also shortened to "K8s," is an **orchestration platform**. Rather than replacing container engines like Docker, it integrates with them and extends their capabilities — relying on the same underlying virtualization models (hypervisors and containers) and coordinating them at a larger scale. See [Concepts](../concepts.md#docker-and-kubernetes-at-a-glance) for what that looks like conceptually (horizontal scaling, self-healing, automated rollouts/rollbacks, etc.).

!!! tip "Security module"
    [Kubernetes Hardening (TryHackMe)](https://tryhackme.com/module/kubernetes-hardening)

## In this section

| Page | Description |
|---|---|
| [Basics](basics.md) | minikube and the core `kubectl` commands for working with a local cluster. |

!!! note "Planned but not yet documented"
    The source notes for this category include placeholder headings for **Cluster Architecture**, **Workload & Scheduling**, **Services & Networking**, **Storage**, and **Troubleshooting** — none of them have content yet, so no pages have been created for them. They'll be added here once real notes exist for each topic.
