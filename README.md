# DOCKER-CC

# Table of Contents
- [DOCKER-CC](#docker-cc)
- [Table of Contents](#table-of-contents)
- [What is Docker](#what-is-docker)
- [Commands](#commands)
  - [Some Commands](#some-commands)
  - [Docker CLI - Running and stopping containers](#docker-cli---running-and-stopping-containers)
  - [Docker CLI - Cleaning up](#docker-cli---cleaning-up)
  - [Docker CLI - Limits](#docker-cli---limits)
  - [Docker CLI - Attach to a container](#docker-cli---attach-to-a-container)
  - [Docker CLI - Building](#docker-cli---building)
  - [Docker CLI - Tagging](#docker-cli---tagging)
  - [Run container with port mapping](#run-container-with-port-mapping)
- [Dockerfile](#dockerfile)

# What is Docker
Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications, whether on laptops, data center VMs, or the cloud.

# Commands
## Some Commands

`docker info` - Display system-wide information

`docker version` - Show the Docker version information

`docker login` - Register or log in to a Docker registry server

`docker help` - List Docker commands

`docker images` - List images

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

## Docker CLI - Cleaning up

`docker rm <container>` - Remove one or more containers

`docker rmi <image>` - Remove one or more images

`docker rm $(docker ps -a -q)` - Remove all stopped containers

`docker system prune -a` - Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes

## Docker CLI - Limits

`docker run --memory=<memory>` - Memory limit **example:** `docker run --memory="256m" nginx`

`docker run --cpus=<cpus>` - CPU limit **example:** `docker run --cpus=".5" nginx`

## Docker CLI - Attach to a container

`docker container exec -it <container> bash` - Attach to a container **example:** `docker container exec -it mycontainer bash` and it will open a bash shell in the container

If you want to exit the container, you can use `exit` command.

## Docker CLI - Building

`docker build -t <image-name> .` - Build an image from a Dockerfile in the current directory **example:** `docker build -t myimage .`

`docker build -t <image-name> -f <path>` - Build an image from a Dockerfile in the specified path **example:** `docker build -t myimage -f /path/to/dockerfile`

## Docker CLI - Tagging
The tag is usually used to identify the version of the image.

`docker tag <image-name> <tag>` - Tag an existing image

![Tagging](images/tagging1.png)

## Run container with port mapping

Run a container with port mapping as shown in the following image:

![Run container](images/run-container.png)

# Dockerfile
Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

1. **Example** of Dockerfile:

    ```
        FROM nginx:alpine
        COPY . /usr/share/nginx/html
    ```

    **Explanation**:

    - The **FROM** command tells Docker which image you base your image on. You always start with something already existing. In this case, we use the alpine version of the official nginx image from Docker Hub.
    - The **COPY** command copies everything from the current folder into the directory /usr/share/nginx/html **in the container**.

    **Build:**
    
    Build the image with the following command:

    ```
        docker build -t webserver-image:v1 .
    ```

    *`Note:` The dot at the end of the command is important. It tells Docker to look for the Dockerfile in the current directory.*

    **Run:**

    Run the container with the following command:

    ```
        docker run -d -p 8080:80 webserver-image:v1
    ```

    **Display:**

    Open a browser and go to http://localhost:8080

2. Example of Dockerfile with multiple commands:

    ```
        FROM alpine
        RUN apk add --update nodejs nodejs-npm
        COPY . /src
        WORKDIR /src
        RUN npm install
        EXPOSE 8080
        ENTRYPOINT ["node", "./app.js"]
    ```

    ![Dockerfile](images/dockerfile1.png)

    **Explanation**:
    
    - The **RUN** command executes a command in a new layer and creates a new image. In this case, we use it to install nodejs and npm.

    - The **COPY** command copies everything from the current folder into the directory /src **in the container**.

    - The **WORKDIR** command sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD commands that follow it in the Dockerfile.
  
    - The **RUN** command executes a command in a new layer and creates a new image. In this case, we use it to install the dependencies of the application.
    
    - The **EXPOSE** command informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.

    - The **ENTRYPOINT** command allows you to configure a container that will run as an executable.

    


