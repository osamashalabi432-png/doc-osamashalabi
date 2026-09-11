# Ivanti Patch Management — Patching Workflow

## Getting started with patch management

The **cloud workflow** is the primary workflow for performing patch management. All configuration and assessment activities are performed within the cloud (Ivanti Neurons), while the actual scans and deployments are performed by agents installed on the managed devices — see [Cloud Platform](cloud-platform.md).

*(A figure in the original notes shows this end-to-end: "A" = agent devices, numbered steps 1–5 = the cloud workflow, reproduced as a table below.)*

| Step | Action |
|---|---|
| 1 | Create a custom policy. |
| 2 | Create a patch configuration and associate it with your policy. |
| 3 | Wait for the changes to propagate to your agent devices. |
| 4 | Agents scan for and deploy patches on the managed devices. |
| 5 | Scan and deployment results are reported to Ivanti Neurons. |

The important part of this sequence is the hand-off at step 3: everything before it is defined centrally in the cloud, and everything from step 4 onward runs locally on the endpoint via the [agent](agents.md) — the cloud never touches the device directly, it just pushes down configuration and waits for the agent to report back.
