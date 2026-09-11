# Podman — Migration

Podman can migrate a running container from one host to another by checkpointing it, copying the checkpoint archive over, and restoring it on the destination — the container must be checkpointed first.

On the **source** system, checkpoint the container to an archive and copy it to the destination:

```bash
sudo podman container checkpoint <container_id> -e /tmp/checkpoint.tar.gz
scp /tmp/checkpoint.tar.gz <destination_system>:/tmp
```

On the **destination** system, restore the container from that archive:

```bash
sudo podman container restore -i /tmp/checkpoint.tar.gz
```
