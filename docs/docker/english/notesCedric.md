# Notes Cedric

- Date Created: 2024-10-31
- Date Last Modified: 2024-10-31

---

Table of contents

- [Glossary](#glossary)
- [Docker](#docker)
  - [Docker Origins](#docker-origins)
  - [What are CGroups?](#what-are-cgroups)
  - [What are namespaces?](#what-are-namespaces)
  - [What is Docker?](#what-is-docker)
  - [What is a container?](#what-is-a-container)
  - [Differences between a container and a virtual machine](#differences-between-a-container-and-a-virtual-machine)
  - [Docker commands](#docker-commands)
    - [How to get logs from a container?](#how-to-get-logs-from-a-container)
    - [How to run a specific image on arm64?](#how-to-run-a-specific-image-on-arm64)
    - [How to filter docker processes?](#how-to-filter-docker-processes)
- [Dockerfile](#dockerfile)
  - [Dockerfile Basic Commands](#dockerfile-basic-commands)


## Glossary

- **Kernel**: The core of the operating system, responsible for managing hardware resources and providing basic services to applications.

---

## Docker


### Docker Origins

Docker is built on top of a kernel feature called "namespaces" and a Linux capability called "control groups" (cgroups).

#### What are CGroups?

CGroups are a Linux kernel feature that limits, accounts for, and isolates the resource usage of a collection of processes.

It limits the resources that a container can use. Like: CPU, memory, disk I/O, network, etc.

#### What are namespaces?

Namespaces are a feature of the Linux kernel that isolates the global system resources from each other.

Resources are isolated in the following ways:

- PID: Process IDs
- MNT: Mount points
- UTS: Hostname and NIS domain name
- IPC: Inter-process communication
- NET: Network devices, stacks, and ports
- USER: User and group IDs

### What is Docker?

Docker is a platform for developing, shipping, and running applications with containers.

It is built on top of a kernel feature called "namespaces" and a Linux capability called "control groups" (cgroups).

#### What is a container?

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

#### Differences between a container and a virtual machine

##### **Containers**

- Share the kernel of the host machine with other containers.
- Are more lightweight and have lower overhead than virtual machines.
- Are better optimized for application execution.

##### **Virtual Machines**

- Have their own isolated kernel.
- Are more resource-intensive and have higher overhead than containers.
- Are better optimized for hardware virtualization.

### Docker commands

- `docker ps`: List all running containers.
- `docker ps -a`: List all containers, including stopped ones.
- `docker inspect <container_id>`: Display detailed information about a container.


#### How to get logs from a container?
`docker logs <container_id>`

- Logs will be displayed in the terminal, to save them in a file, you can use 
`docker logs <container_id> > <file_name>.log`

- If you want to follow the logs in real time, you can use `docker logs -f <container_id>`

#### How to run a specific image on arm64?
`docker run --platform linux/arm64 <image_name>`

#### How to filter docker processes?
Different filters:
- `docker ps --filter "status=running"`
- `docker ps --filter "name=my_container"`
- `docker ps --filter "ancestor=<image_name>"`
- `docker ps --filter "volume=<volume_name>"`
- `docker ps --filter "publish=<port>"`
- `docker ps --filter "health=healthy"`


# Dockerfile

## Dockerfile Basic Commands
- `FROM <image_name>`: Specifies the base image to use for the container.
- `RUN <command>`: Executes a command in the container.
- `COPY <source> <destination>`: Copies files or directories from the host machine to the container.
- `CMD ["<command>"]`: Specifies the command to run when the container starts.

## Good practice:
- Make the most general and longest steps first (use of cache !)
- If the commands are long, assign their own layer.
- Use `&&` and `\` to combine similar commands.

## About docker cache:
- Docker caches the layers of the image. Meaning that if you run `docker build` again, it will use the cached layers instead of refetching them. So each time you run `docker build`, it will be faster.
- If you don't want to use the cache, you can use the `--no-cache` flag with the `docker build` command.
- If you want to force Docker to refetch all the layers, you can use the `--pull` flag with the `docker build` command.



### About ENTRYPOINT:
- The `ENTRYPOINT` instruction is used to specify the command to run when the container starts.
- It is used to configure the container to run a specific command.
- When to use ? If you want to configure the container to run a specific command, you should use `ENTRYPOINT`.

### About the CMD instruction
- The `CMD` instruction is used to specify the command to run when the container starts.
- If you want to run multiple commands, you can use `CMD ["<command1>", "argument1"]`.
- If you want to run a command in the background, you can use `CMD ["<command>"]`.
- It can only be used once in the Dockerfile.


### About ADD instruction:
- The `ADD` instruction is used to copy files or directories from the host machine to the container.
- It can be used multiple times in the Dockerfile.

## Building a docker image
Building an image manually, let's say you take an image and it doesn't have the dependencies you need. This image uses `yum` to install dependencies.
You install a new dependency, let's say `wget`. 

### How do you check the differences between the parent image and the current image?
- `docker diff <container_id>`: This will show you the differences between the parent image and the current image. All files that have been added, modified or deleted will be displayed in output.

## How to create a new image manually from a container?
Taking the example of the previous section, you can create a new image from a container by using the following command and it should have the new dependency. 

Let's say the container id is `my_container_is_cool`.

- In this case, the new image will be named `bob_has_wget`:
`docker commit my_container_is_cool bob_has_wget`

## How to create a new image from a Dockerfile?
Command: `docker build -t <image_name> .`

- When you use the `.` at the end of the command, it means that the Dockerfile is in the current directory.
- The `-t` flag is used to name the image.
- Each time that you run the command, a new layer is created.

## Docker Networking

### Docker Networking Model
- The containers don't have a public IPv4 address
- They have a private address
- The services must be exposed port by port
- The container ports must be mapped to the host ports to avoid conflicts
- At startup, Docker creates a virtual interface docker0 on the host with a random private address

### Docker Networking Commands
- `docker network ls`: List all networks
- `docker network inspect <network_name>`: Display detailed information about a network
- `docker network create <network_name>`: Create a new network
- `docker network rm <network_name>`: Remove a network

### Docker Port 
- You can use the `docker port <container_id>` command to get the port mapping of a container.

To expose a port, you can use the `-p` flag with the `docker run` command.

- `docker run -p <host_port>:<container_port> <image_name>`: Map the container port to the host port
- `docker run -P <image_name>`: Map all the container ports to the host ports randomly