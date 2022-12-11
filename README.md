# DOCKER-CC

## What is Docker
Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications, whether on laptops, data center VMs, or the cloud.

## Some Commands

`docker info` - Display system-wide information

`docker version` - Show the Docker version information

`docker login` - Register or log in to a Docker registry server

`docker help` - List Docker commands

## Docker CLI - Running and stopping containers

**< image >** The name of the image as you find it in the container registry. **< container >** - The name or ID of the container

`docker pull <image>` - Pull an image or a repository from a registry

`docker run <image>` - Run a command in a new container

`docker run -d <image>` - Run a container in the background and print the new container ID

`docker start <container>` - Start one or more stopped containers

`docker ps` - List running containers

`docker ps -a` - List all containers

`docker stop <container>` - Stop one or more running containers

`docker restart <container>` - Restart one or more containers

`docker kill <container>` - Kill one or more running containers

`docker image inspect <image>` - Display detailed information on one or more images

## Docker CLI - Limits

`docker run --memory=<memory>` - Memory limit **example:** `docker run --memory="256m" nginx`

`docker run --cpus=<cpus>` - CPU limit **example:** `docker run --cpus=".5" nginx`



