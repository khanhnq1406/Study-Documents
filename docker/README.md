# Docker summary

## Table of contents

- [Docker summary](#docker-summary)
  - [Table of contents](#table-of-contents)
  - [Docker overview](#docker-overview)
    - [What is Docker?](#what-is-docker)
    - [Docker architecture](#docker-architecture)
  - [Images \& Containers](#images--containers)
    - [What are containers](#what-are-containers)
    - [What is an image?](#what-is-an-image)
    - [Dockerfile](#dockerfile)
    - [Layers](#layers)
  - [Key commands](#key-commands)
    - [docker build](#docker-build)
      - [Usage](#usage)
      - [Options](#options)
    - [docker run](#docker-run)
      - [Usage](#usage-1)
      - [Some options](#some-options)
  - [Storage](#storage)
    - [Volumes and Bind Mounts](#volumes-and-bind-mounts)
      - [What are Docker Volumes?](#what-are-docker-volumes)
      - [Types of Docker Volumes](#types-of-docker-volumes)
      - [Bind mounts](#bind-mounts)
      - [Noted](#noted)
      - [Choose the right type of mount](#choose-the-right-type-of-mount)
    - [Networking](#networking)
      - [Published ports](#published-ports)
      - [Use user-defined bridge networks](#use-user-defined-bridge-networks)
      - [Noted](#noted-1)
  - [Docker compose](#docker-compose)
    - [Overview](#overview)
  - [References](#references)

## Docker overview

### What is Docker?

Docker is an open platform for developing, shipping, and running applications. Docker provides the ability to package and run an application in a loosely isolated environment called a container.

### Docker architecture

<figure>
 <img src="https://docs.docker.com/get-started/images/docker-architecture.png">
 <figcaption>
     Source:
     <a href="https://docs.docker.com/get-started/overview/">Docker Overview</a>
 </figcaption>
</figure>

## Images & Containers

### What are containers

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

<figure>
 <img src="https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg.png">
 <figcaption>
     Source:
     <a href="https://www.docker.com/resources/what-container/">What is a container?</a>
 </figcaption>
</figure>

### What is an image?

A running container uses an isolated filesystem. This isolated filesystem is provided by an image, and the image must contain everything needed to run an application - all dependencies, configurations, scripts, binaries, etc. The image also contains other configurations for the container, such as environment variables, a default command to run, and other metadata.

A Docker image is like a snapshot in other types of VM environments. Containers need a runnable image to exist. Containers are dependent on images, because they are used to construct runtime environments and are needed to run an application.

### Dockerfile

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.

<a href="https://docs.docker.com/engine/reference/builder/"> Dockerfile reference</a>

<a href="https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index">Dockerfile cheat sheet</a>

```
FROM node
WORKDIR /app
COPY . /app
RUN npm install
ARG port=80
EXPOSE ${port}
CMD node server.js
```

### Layers

The order of Dockerfile instructions matters. A Docker build consists of a series of ordered build instructions. Each instruction in a Dockerfile roughly translates to an image layer. The following diagram illustrates how a Dockerfile translates into a stack of layers in a container image.

<img src="https://docs.docker.com/build/guide/images/layers.png"/>

## Key commands

<a href="https://docs.docker.com/get-started/docker_cheatsheet.pdf"> Docker CLI cheatsheet</a>

<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--4BzDJqxv--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn.hashnode.com/res/hashnode/image/upload/v1608978415158/0zm0Ied31.jpeg"/>

### docker build

#### Usage

`docker build [OPTIONS] PATH | URL | -`

#### Options

| **Options**             | **Short** | **Description**                                                                   |
| ----------------------- | --------- | --------------------------------------------------------------------------------- |
| --add-host              |           | Add a custom host-to-IP mapping (host:ip)                                         |
| --build-arg             |           | Set build-time variables                                                          |
| --cache-from            |           | Images to consider as cache sources                                               |
| --cgroup-parent         |           | Optional parent cgroup for the container                                          |
| --compress              |           | Compress the build context using gzip                                             |
| --cpu-period            |           | Limit the CPU CFS (Completely Fair Scheduler) period                              |
| --cpu-quota             |           | Limit the CPU CFS (Completely Fair Scheduler) quota                               |
| --cpu-shares            | -c        | CPU shares (relative weight)                                                      |
| --cpuset-cpus           |           | CPUs in which to allow execution (0-3, 0,1)                                       |
| --cpuset-mems           |           | MEMs in which to allow execution (0-3, 0,1)                                       |
| --disable-content-trust |           | Skip image verification                                                           |
| --file                  | -f        | Name of the Dockerfile (Default is PATH/Dockerfile)                               |
| --force-rm              |           | Always remove intermediate containers                                             |
| --iidfile               |           | Write the image ID to the file                                                    |
| --isolation             |           | Container isolation technology                                                    |
| --label                 |           | Set metadata for an image                                                         |
| --memory                | -m        | Memory limit                                                                      |
| --memory-swap           |           | Swap limit equal to memory plus swap: -1 to enable unlimited swap                 |
| --network               |           | API 1.25+ Set the networking mode for the RUN instructions during build           |
| --no-cache              |           | Do not use cache when building the image                                          |
| --platform              |           | API 1.38+ Set platform if server is multi-platform capable                        |
| --pull                  |           | Always attempt to pull a newer version of the image                               |
| --quiet                 | -q        | Suppress the build output and print image ID on success                           |
| --rm                    |           | Remove intermediate containers after a successful build                           |
| --security-opt          |           | Security options                                                                  |
| --shm-size              |           | Size of /dev/shm                                                                  |
| --squash                |           | API 1.25+ experimental (daemon) Squash newly built layers into a single new layer |
| --tag                   | -t        | Name and optionally a tag in the name:tag format                                  |
| --target                |           | Set the target build stage to build.                                              |
| --ulimit                |           | Ulimit options                                                                    |

### docker run

#### Usage

The basic docker run command takes this form:

`$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`

The docker run command must specify an IMAGE to derive the container from. An image developer can define image defaults related to:

- detached or foreground running
- container identification
- network settings
- runtime constraints on CPU and memory

#### Some options

| **Options**       | **Description**                                  |
| ----------------- | ------------------------------------------------ |
| -e, --env list    | Set environment variables                        |
| -i, --interactive | Keep STDIN open even if not attached             |
| -t, --tty         | Allocate a pseudo-TTY                            |
| --rm              | Automatically remove the container when it exits |
| -v, --volume list | Bind mount a volume                              |

## Storage

### Volumes and Bind Mounts

#### What are Docker Volumes?

Volumes are the preferred mechanism for persisting data generated by and used by Docker containers. While bind mounts are dependent on the directory structure and OS of the host machine, volumes are completely managed by Docker.

#### Types of Docker Volumes

**Named Volumes**

Named volumes have a user-defined name, making them easy to identify, manage, and share among multiple containers.

To create a named volume in Docker, run:
`docker volume create my_named_volume`

**Anonymous Volumes**

Unlike named volumes, anonymous volumes don’t have a user-defined name. Instead, Docker automatically creates them when you create a container and assign a unique ID to the volume.

To create a container with an anonymous volume, run:
`docker run -v /data image_name`

#### Bind mounts

When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine.

#### Noted

If you bind mount a folder that contains an existing folder in the container, you will need to create an anonymous volume.

For example, if you run `npm install` to install packages into `/app/node_modules` in the Dockerfile, you will need to create a volume `-v /app/node_modules` when running the container.

#### Choose the right type of mount

<img src="https://docs.docker.com/storage/images/types-of-mounts-bind.webp"/>

- **Volumes** are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.

- **Bind mounts** may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.

- **tmpfs mounts** are stored in the host system's memory only, and are never written to the host system's filesystem.

### Networking

#### Published ports

By default, when you create or run a container using docker create or docker run, the container doesn't expose any of its ports to the outside world. Use the --publish or -p flag to make a port available to services outside of Docker. This creates a firewall rule in the host, mapping a container port to a port on the Docker host to the outside world.

For example:
`docker run -p 8080:80 image_name` -> Map port 8080 on the host to TCP port 80 in the container.

#### Use user-defined bridge networks

Create the alpine-net network:
`docker network create alpine-net`

Create your container. Notice the --network flags. You can only connect to one network during the docker run command, so you need to use docker network connect afterward to connect a container to the bridge network as well.

`docker run --network alpine-net image_name`

`docker network connect bridge container_name`

#### Noted

**I want to connect from a container to a service on the host**

The host has a changing IP address, or none if you have no network access. We recommend that you connect to the special DNS name `host.docker.internal`, which resolves to the internal IP address used by the host.

**I want to connect from a container to a service on other container**

When two containers are connected within the same network, you can use the container name for DNS.
When two container are connected different network, you can you the IP address of container for DNS.

## Docker compose

### Overview

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

Using Compose is essentially a three-step process:

1. Define your app's environment with a Dockerfile so it can be reproduced anywhere.
2. Define the services that make up your app in a compose.yaml file so they can be run together in an isolated environment.
3. Run docker compose up and the Docker compose command starts and runs your entire app.

A compose.yaml looks like this:

```
services:
  web:
 build: .
 ports:
   - "8000:5000"
 volumes:
   - .:/code
   - logvolume01:/var/log
 depends_on:
   - redis
  redis:
 image: redis
volumes:
  logvolume01: {}
```

## References

<https://docs.docker.com/get-started/overview/>

<https://docs.docker.com/engine/reference>

<https://docs.docker.com/get-started/docker_cheatsheet.pdf>

<https://docs.docker.com/engine/>

<https://docs.docker.com/compose/>
