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
  - [Docker CLI - Volume](#docker-cli---volume)
  - [Docker CLI - Compose](#docker-cli---compose)
  - [Docker CLI - Compose V2](#docker-cli---compose-v2)
  - [Run container with port mapping](#run-container-with-port-mapping)
  - [Run container with volume mapping](#run-container-with-volume-mapping)
    - [Mapping to a local folder](#mapping-to-a-local-folder)
- [Dockerfile](#dockerfile)
- [Docker extension for VS Code](#docker-extension-for-vs-code)
  - [Docker build image VS Code](#docker-build-image-vs-code)
  - [Docker run image VS Code](#docker-run-image-vs-code)
- [Persisting data](#persisting-data)
  - [Docker volumes](#docker-volumes)
  - [Trying a volume](#trying-a-volume)
  - [Remove a volume while a container is using it](#remove-a-volume-while-a-container-is-using-it)
- [Docker Compose](#docker-compose)
  - [Example](#example)
  - [Use cases](#use-cases)
  - [Docker compose file](#docker-compose-file)
  - [Sample app](#sample-app)
    - [Docker compose file](#docker-compose-file-1)
    - [Networks](#networks)
    - [Volumes shared between services](#volumes-shared-between-services)
    - [Volumes for each service](#volumes-for-each-service)
  - [Docker compose features](#docker-compose-features)
    - [Resource limits](#resource-limits)
    - [Environment variables](#environment-variables)
    - [Networking](#networking)
    - [Dependence](#dependence)
    - [Volumes - named volumes](#volumes---named-volumes)
    - [Volumes](#volumes)
    - [Restart policy](#restart-policy)

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

## Docker CLI - Volume

`docker create volume <volume-name>` - Create a volume

`docker volume ls` - List volumes

`docker volume inspect <volume-name>` - Inspect a volume

`docker volume rm <volume-name>` - Remove a volume

`docker volume prune` - Remove all unused volumes

## Docker CLI - Compose

`docker compose build` - Build or rebuild services

`docker compose build --no-cache` - Build or rebuild services without using cache

`docker compose build -f <path>` - Build or rebuild services specifying the path to the docker-compose.yml file

`docker compose start` - Start services

`docker compose stop` - Stop services

`docker compose up -d` - Create and start containers, detached mode

`docker compose up -d -e <environment_variable>` - Create and start containers, detached mode, specifying an environment variable. **example:** `docker compose up -d -e DEBUG=0`

`docker compose -p <project-name> up -d` - Create and start containers, detached mode, specifying the project name. It can run multiple projects at the same time

`docker compose ps` - List containers

`docker compose rm` - Remove stopped containers

`docker compose down` - Stop and remove containers, networks, images, and volumes

`docker compose logs` - View output from containers

`docker compose exec <container> bash` - Attach to a container


## Docker CLI - Compose V2

`docker compose --project-name <project-name> up -d` - Create and start containers, detached mode, specifying the project name. It can run multiple projects at the same time

`docker compose -p <project-name> up -d` - shortcut for the previous command

`docker compose ls` - List containers

`docker compose cp <container>:/path/to/file /path/to/paste` - Copy files/folders from a container to the local filesystem

`docker compose cp /path/to/file <container>:/path/to/paste` - Copy files/folders from the local filesystem to a container

## Run container with port mapping

Run a container with port mapping as shown in the following image:

![Run container](images/run-container.png)

## Run container with volume mapping

Run a container with volume mapping as shown in the following image:

![Run container](images/run-container-volume.png)

### Mapping to a local folder

Sometimes you want to map a volume to a local folder. This is useful for development purposes to test your application. **Don't use it in production**

![Run container](images/run-container-volume2.png)

# Dockerfile
Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

1. **Example** of Dockerfile:

    ```dockerfile
        FROM nginx:alpine
        COPY . /usr/share/nginx/html
    ```

    **Explanation**:

    - The **FROM** command tells Docker which image you base your image on. You always start with something already existing. In this case, we use the alpine version of the official nginx image from Docker Hub.
    - The **COPY** command copies everything from the current folder into the directory /usr/share/nginx/html **in the container**.

    **Build:**
    
    Build the image with the following command:

    ```bash
        docker build -t webserver-image:v1 .
    ```

    *`Note:` The dot at the end of the command is important. It tells Docker to look for the Dockerfile in the current directory.*

    **Run:**

    Run the container with the following command:

    ```bash
        docker run -d -p 8080:80 webserver-image:v1
    ```

    **Display:**

    Open a browser and go to http://localhost:8080

2. Example of Dockerfile with multiple commands:

    ```dockerfile
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

    
# Docker extension for VS Code

![Docker extension](images/docker-extension.png)

If we want to add a Dockerfile to our project, we can use the Docker extension for VS Code. It will create a Dockerfile for us and we can add the commands we need.

If we open the command palette and type `Docker: Add Docker Files to Workspace`, and we follow the steps, we will get a generated Dockerfile and other docker files for a Django project, **See django_test_project/**

## Docker build image VS Code

Now we can search for `Docker: Build Image` in the command palette and we can build the image.

## Docker run image VS Code

Now we can search for `Docker: Run Image` in the command palette andthen select the image we want to run.

# Persisting data

## Docker volumes

![Containers are ephimeral](images/persistent1.png)

Docker volumes are the preferred mechanism for persisting data generated by and used by Docker containers. While bind mounts are dependent on the directory structure of the host machine, volumes are completely managed by Docker. Volumes have several advantages over bind mounts:

- Volumes are easier to back up or migrate than bind mounts.

- You can manage volumes using Docker CLI commands or the Docker API.

- Volumes work on both Linux and Windows containers.

- Volumes can be more safely shared among multiple containers.

- Volume drivers allow you to store volumes on remote hosts or cloud providers, to encrypt the contents of volumes, or to add other functionality.

![Volumes](images/persistent2.png)

## Trying a volume

1. Create a volume:

    ```bash
        docker volume create myvol
    ```

2. Run a container with the volume:

    ```bash
        docker run -d --name voltest -v myvol:/app nginx:latest
    ```

3. Attach to the container:

    ```bash
        docker exec -it voltest bash
    ```

4. Install nano:

    ```bash
    # Inside the container
        apt-get update
        apt-get install nano
    ```

5. Create a file:

    ```bash
    # Inside the container
        cd /app
        nano test.txt # Write 'Hello volume!' and save
    ```

6. Exit the container:

    ```bash
    # Inside the container
        exit
    ```

7. Remove the container:

    ```bash
        docker rm -f voltest
    ```

7. Run a new container with the same volume:

    ```bash
        docker run -d --name voltest2 -v myvol:/app nginx:latest
    ```

8. Attach to the container:

    ```bash
        docker exec -it voltest2 bash
    ```

9. Check the file:

    ```bash
    # Inside the container
        cd /app
        cat test.txt
    ```

    You should see the text **'Hello volume!'** because the volume is persistent and it is stored and shared between the containers.

## Remove a volume while a container is using it

If we try to remove a volume while a container is using it, we will get an error:

```bash
    docker volume rm myvol
    Error response from daemon: remove myvol: volume is in use - [d9f9f9f9f9f9f9f9f]
```

# Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

![Docker compose](images/compose1.png)

</br>

## Example

![Docker compose](images/compose2.png)

</br>

## Use cases

![Docker compose](images/compose3.png)

</br>

## Docker compose file

Docker compose file is located inside a folder: and you can run multiple containers specifying a name for each one.

![Docker compose](images/compose4.png)

</br>

## Sample app

![Docker compose](images/compose5.png)
</br>

### Docker compose file

![Docker compose](images/compose0.png)
![Docker compose](images/compose6.png)

- Definition of 3 services: backend, db and frontend.

    **db** uses an image from docker hub, **backend** and **frontend** use images that we have built in the Dockerfile.

![Docker compose](images/compose7.png)

**Backend**

    - **build** is the path to the Dockerfile.
    - **context** is the path to the folder where the Dockerfile is located.

![Docker compose](images/compose8.png)

### Networks

    We are defining two networks, one is private and the other is public.

**backend** is using both the private and public network, so it can communicate with **frontend** and **db**.
**frontend** is using the public network.
**db** is using the private network.

![Docker compose](images/compose9.png)

### Volumes shared between services

    We are defining two volumes that are shared between the services. 

![Docker compose](images/compose10.png)

### Volumes for each service

    We are also defining volumes at the service level that are not shared between the services.

![Docker compose](images/compose11.png)

## Docker compose features

### Resource limits

It's a good practice to set resource limits for each service.

![Docker compose](images/compose12.png)

### Environment variables

    We can set environment variables for each service.

![Docker compose](images/compose13.png)

We can reference an environment variable using the syntax **${VARIABLE}**.

This way we can set a variable in the machine or server where we are running the containers and we can reference it in the docker compose file.

Also, we can set a file with environment variables and reference it in the docker compose file. 

> ⚠️ Note: this file must be in the same folder as the docker compose file with the name **.env** and the docker compose will automatically read the variables from that file.

![Docker compose](images/compose14.png)

### Networking

By default, all containers specified in the docker compose file will see each other and they will be able to communicate with each other **using the container name as the host_name**.

The web service shown in the image below can be accessed outside the docker compose network using the port 8080.

Web is listening inside the docker network in port 80.

DB **only exposes one port number**, that's the internal port number, and it can't be accessed outside the docker compose network.

![Docker compose](images/compose15.png)

If we have multiple services, we can restrict the communication between them using the **networks** section.

In this example we are defining two **networks: Frontend y backend**, the proxy can communicate with the app using the **Frontend** network and the app can communicate with the db service using the **backend** network.

However, the proxy can't communicate with the db service, because it is not in the same network.

![Docker compose](images/compose16.png)

### Dependence

We can define a dependence between services using the **depends_on** section. This way, the service that depends on another service will start after the other service is running.

![Docker compose](images/compose17.png)

### Volumes - named volumes

We can define named volumes using the **volumes** section. They can be used by all the services that are defined in the docker compose file.

If we want to use the volume in a specific service, we can map the volume to a specific path in the service.

![Docker compose](images/compose18.png)

### Volumes

We can also define volumes at the service level. This mapping can't be shared between services.

![Docker compose](images/compose19.png)

### Restart policy

We can define a restart policy for each service using the **restart** section.

![Docker compose](images/compose20.png)

