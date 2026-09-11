# Ivanti Patch Management — Cloud Platform

## Cloud workflow

The cloud workflow is the primary workflow for performing patch management. The split is:

- **All configuration and assessment activities are performed within the cloud** (Ivanti Neurons) — this is where policies and patch configurations are created and managed.
- **The actual scans and deployments are performed by agents** installed on the managed devices.

So the cloud platform decides *what* should happen (policy, patch selection), and the [agent](agents.md) on each endpoint is what actually carries it out and reports back. See [Patching Workflow](patching-workflow.md) for the full step sequence.
