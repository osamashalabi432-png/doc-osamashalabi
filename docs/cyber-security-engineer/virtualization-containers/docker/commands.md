# Docker — Commands

Core Docker CLI commands for day-to-day container and image management.

## Version and general info

Check what version of Docker is installed, and get a general summary of the Docker installation (storage driver, number of containers/images, etc.):

```bash
docker -v
```

```bash
docker info
```

## Listing containers and images

List running containers:

```bash
docker ps
```

or, equivalently:

```bash
docker container ls
```

List images available locally:

```bash
docker images ps
```

## Running containers

Run a container in the background (detached), so it doesn't tie up the terminal:

```bash
docker container run -d hello-world
```

Give the container a custom name instead of Docker's auto-generated one, by appending `:name`:

```bash
docker container run hello-world:name
```

Publish a container on a port so it's reachable from outside the host — this example runs nginx detached and maps host port 80 to container port 80:

```bash
docker container run --detach --publish 80:80 --name n1 nginx
```

Stop a running container:

```bash
docker container stop <container name>
```

Open an interactive shell inside a running container — useful for troubleshooting or inspecting what's happening on the inside:

```bash
docker exec -it <docker name> <command(bash)>
```

## Inspecting containers

Get detailed metadata about a container or image (config, mounts, network settings, etc.):

```bash
docker inspect <docker name>
```

Get all logs for a container:

```bash
docker logs <image ID>
```

Get a live view of a container's resource usage (CPU, memory, network I/O):

```bash
docker stats <image ID>
```

Get just the container's IP address from its network settings:

```bash
docker inspect --format='{{range.NetworkSettings.Networks}}{{IPAddress}}{{end}}' <image ID>
```

!!! note "Docker tags"
    A **Docker tag** is a reference to a specific docker image (e.g. a version or variant of it).

## Building images with a Dockerfile

Docker builds images automatically by reading instructions from a **Dockerfile** — a text file containing every command a user could otherwise type manually to assemble an image.

The first instruction in a Dockerfile is normally `FROM`, which decides what base OS/image the new image will build on top of.

Build an image from a Dockerfile in the current directory (the `.` means "build context is here"):

```bash
docker build .
```

## Networking

List all Docker networks:

```bash
docker network ls
```

Inspect a specific network:

```bash
docker network inspect <docker name>
```

Create a network with a chosen driver (network type):

```bash
docker network create --driver <type of network> <name of the network>
```

Run a container attached to a specific network:

```bash
docker run --network=<network name> -d -it Ubuntu
```

Create a network with an explicit subnet:

```bash
docker network create --driver <type of network> --subnet 172.25.0.0/16 <network name>
```

## Docker Compose

**Docker Compose** is a tool for defining and running multi-container Docker applications. Instead of starting each container by hand, you describe all of an application's services in a single **YAML** ("Yet Another Markup Language") file, then bring the whole stack up or down together.

Check the installed Compose version:

```bash
docker-compose -v
```
