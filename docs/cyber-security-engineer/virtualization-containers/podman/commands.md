# Podman — Commands

## Version and info

Check the installed Podman version:

```bash
podman -v
```

Get general information about the Podman installation:

```bash
podman info
```

## Images

Pull an image:

```bash
podman pull <image name>
```

List images:

```bash
podman images ps
```

## Running containers

Run a container from an image:

```bash
podman run <image name>
```

Give the container a custom name:

```bash
podman run --name <new name> <image>
```

Run detached (in the background):

```bash
podman run -d <image>
```

Publish a container's port to the host:

```bash
podman run -p <internal port>:<external port>/<TCP or UDP> <image>
```

## Listing containers

List running containers:

```bash
podman container ps
```

List all containers, including stopped ones:

```bash
podman container ps -a
```

## Logs

View a container's logs:

```bash
podman logs <image ID>
```

## Checkpoint and restore

Freeze and save ("checkpoint") a running container's state to disk:

```bash
sudo podman container checkpoint <container_id>
```

Restore a container from a checkpoint — this only works for a container that was previously checkpointed:

```bash
sudo podman container restore <container_id>
```

See [Migration](migration.md) for using checkpoint/restore to move a container between hosts.

## Stopping and removing containers

Stop a container:

```bash
podman stop <container ID>
```

Remove a container:

```bash
podman rm <container ID>
```
