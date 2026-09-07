# Docker Labs: Container Administration & Image Management

This README contains hands-on Docker labs covering container administration, image management, Dockerfile creation, image optimization, and local cleanup.

---

# Lab 1.1: Container Administration

## Goal

Provision a local container, interact with its filesystem, and manage its lifecycle.

## Step 1: Access the Local Command Line

Choose the appropriate terminal for your operating system:

- **macOS:** Open the **Terminal** application.
- **Windows:** Open **PowerShell** as an administrator.

## Step 2: Run a Background Container

Pull and run an Nginx web server in detached mode. Detached mode means the container runs in the background.

Run:

    docker run -d --name local-web-server nginx:alpine

Docker will output a long string of characters. This is the unique **Container ID** identifying the container that was created.

## Step 3: Run an Interactive Container

Launch an Ubuntu container and attach your terminal to it interactively.

Run:

    docker run -it ubuntu bash

Once inside the container, execute:

    ls

The `ls` command displays the isolated Linux filesystem running inside the Ubuntu container.

To exit the container and return to your local host terminal, run:

    exit

## Step 4: Inspect and Clean Up

List all containers on your host, including containers that have stopped:

    docker ps -a

You should see both the Nginx container and the Ubuntu container.

Remove the Nginx container:

    docker rm -f local-web-server

Remove the Ubuntu container using its Container ID:

    docker rm <UBUNTU_CONTAINER_ID>

Replace `<UBUNTU_CONTAINER_ID>` with the actual Container ID shown by:

    docker ps -a

---

# Lab 1.2: Image Management

## Goal

Author a standard Dockerfile to package a custom application into a Docker image.

## Step 1: Create the Project Directory

Create a new directory for the project and navigate into it:

    mkdir enterprise-web
    cd enterprise-web

## Step 2: Create the Application File

Open your preferred text editor, such as:

- VS Code
- Notepad
- TextEdit

Create a new file named:

    index.html

Add the following HTML:

    <h1>Welcome to the Firatech Enterprise Bootcamp!</h1>
    <p>Running natively on localhost.</p>

Save the file inside the `enterprise-web` directory.

The project should now look like:

    enterprise-web/
    └── index.html

## Step 3: Author the Dockerfile

In the same directory, create a new file named exactly:

    Dockerfile

Do not add a file extension.

Add the following Dockerfile instructions:

    FROM nginx:alpine
    COPY index.html /usr/share/nginx/html/index.html

Save the file.

The project should now look like:

    enterprise-web/
    ├── Dockerfile
    └── index.html

## Step 4: Build the Custom Image

Return to your terminal and make sure you are inside the `enterprise-web` directory.

Build the Docker image:

    docker build -t custom-web-app .

The `-t` option assigns the name `custom-web-app` to the image.

Important: Do not forget the period `.` at the end of the command. The period specifies the current directory as the Docker build context.

## Step 5: Deploy and Verify

Run the container and map port `8080` on your laptop to port `80` inside the container:

    docker run -d -p 8080:80 custom-web-app

The `-p 8080:80` option maps:

- Port `8080` on your laptop
- To port `80` inside the Docker container

Open a web browser such as Chrome, Safari, Edge, or Firefox.

Navigate to:

    http://localhost:8080

You should see:

    Welcome to the Firatech Enterprise Bootcamp!

---

# Lab 1.3: Image Optimization & Local Cleanup

## Goal

Analyze Docker's layer and image system, optimize Dockerfiles, compare image sizes, and execute a local cleanup protocol to reclaim hard-drive space.

## Step 1: Build a Bloated Image

In your text editor, create a new file named:

    Dockerfile.bloated

Add the following unoptimized Dockerfile instructions:

    FROM ubuntu:latest
    RUN apt-get update
    RUN apt-get install -y curl
    CMD ["echo", "Done"]

Build the bloated image:

    docker build -t bloated-image -f Dockerfile.bloated .

The `-f Dockerfile.bloated` option tells Docker to use `Dockerfile.bloated` instead of the default `Dockerfile`.

## Step 2: Build an Optimized Image

Create another file named:

    Dockerfile.optimized

Use a lightweight Alpine base image and combine the package-management commands:

    FROM alpine:latest
    RUN apk update && apk add curl
    CMD ["echo", "Done"]

Build the optimized image:

    docker build -t optimized-image -f Dockerfile.optimized .

## Step 3: Analyze the Results

Compare the sizes of the two generated images on your local machine:

    docker images

Look at the `SIZE` column and compare:

- `bloated-image`
- `optimized-image`

The Alpine-based image should generally be significantly smaller than the Ubuntu-based image.

The exact image sizes may vary depending on:

- Docker version
- CPU architecture
- Operating system
- Image version
- Current package versions

The important concept is that choosing a smaller base image can significantly reduce the final image size.

## Step 4: The Cleanup Protocol

When working with Docker on your personal laptop, unused images, stopped containers, networks, and volumes can consume significant disk space.

To remove unused Docker resources, run:

    docker system prune -a --volumes

Docker will display a warning and ask you to confirm the operation.

Type:

    y

Then press **Enter**.

WARNING: The command below can remove unused Docker resources, including:

- Stopped containers
- Unused images
- Unused networks
- Unused volumes

Make sure you do not need any of the unused Docker resources before confirming the cleanup.

---

# Lab Summary

By completing these labs, you should be able to:

- Run Docker containers in detached mode.
- Run Docker containers interactively.
- Interact with a container's filesystem.
- List running and stopped containers.
- Remove Docker containers.
- Create a basic HTML application.
- Write a standard Dockerfile.
- Build a custom Docker image.
- Run a custom Docker image as a container.
- Map host ports to container ports.
- Serve a custom webpage using Nginx.
- Compare Docker image sizes.
- Understand the importance of choosing an appropriate base image.
- Apply basic Dockerfile optimization techniques.
- Clean up unused Docker resources.
- Reclaim local disk space used by Docker.

---

# Useful Docker Commands

| Command | Purpose |
|---|---|
| `docker run` | Create and run a container |
| `docker run -d` | Run a container in detached/background mode |
| `docker run -it` | Run a container interactively |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers, including stopped containers |
| `docker rm` | Remove a container |
| `docker rm -f` | Force-remove a container |
| `docker build` | Build an image from a Dockerfile |
| `docker images` | List Docker images |
| `docker system prune` | Remove unused Docker resources |
| `docker info` | Display information about the Docker installation |
| `docker --version` | Display the installed Docker version |

---

# Prerequisites

Before starting these labs, ensure that Docker is installed and running on your computer.

## Verify Docker Installation

Check the installed Docker version:

    docker --version

You should receive output similar to:

    Docker version XX.X.X, build XXXXXXX

You can also verify that the Docker engine is running:

    docker info

If both commands execute successfully, your Docker environment is ready.

---

# Project Structure

After completing Lab 1.2 and Lab 1.3, your project directory may look like this:

    enterprise-web/
    ├── Dockerfile
    ├── Dockerfile.bloated
    ├── Dockerfile.optimized
    └── index.html

---

# Key Docker Concepts Covered

## Containers

A container is a lightweight, isolated environment used to run applications and their dependencies.

In this lab, you worked with:

- Nginx
- Ubuntu

## Images

A Docker image is a packaged, read-only template used to create containers.

In this lab, you created:

- `custom-web-app`
- `bloated-image`
- `optimized-image`

## Dockerfile

A Dockerfile contains instructions that Docker uses to build an image.

For example:

    FROM nginx:alpine
    COPY index.html /usr/share/nginx/html/index.html

## Port Mapping

Port mapping allows applications running inside containers to be accessed from the host machine.

For example:

    docker run -d -p 8080:80 custom-web-app

This maps:

    Host Port 8080 → Container Port 80

Therefore, the application can be accessed through:

    http://localhost:8080

## Image Optimization

Using a lightweight base image such as Alpine can reduce the size of the resulting Docker image.

The lab compares:

    Ubuntu → bloated-image

with:

    Alpine → optimized-image

This demonstrates why selecting an appropriate base image is an important part of container optimization.

---

# Completion Checklist

Use this checklist to confirm that you have completed the labs:

- [ ] Docker is installed and running.
- [ ] Nginx container was successfully started.
- [ ] Ubuntu container was successfully started.
- [ ] Container filesystem was inspected using `ls`.
- [ ] Containers were listed using `docker ps -a`.
- [ ] Containers were removed successfully.
- [ ] `enterprise-web` project directory was created.
- [ ] `index.html` was created.
- [ ] `Dockerfile` was created.
- [ ] `custom-web-app` image was successfully built.
- [ ] Custom Nginx container was successfully started.
- [ ] Application was accessed through `http://localhost:8080`.
- [ ] `Dockerfile.bloated` was created.
- [ ] `bloated-image` was successfully built.
- [ ] `Dockerfile.optimized` was created.
- [ ] `optimized-image` was successfully built.
- [ ] Image sizes were compared using `docker images`.
- [ ] Docker cleanup was performed using `docker system prune -a --volumes`.

---

# Conclusion

These labs provide a practical introduction to Docker container administration and image management.

You have learned how to:

1. Create and manage containers.
2. Interact with containers using an interactive shell.
3. Build custom Docker images.
4. Package a simple web application using Nginx.
5. Expose containerized applications through host ports.
6. Compare Docker image sizes.
7. Optimize Docker images using lightweight base images.
8. Clean up unused Docker resources.

After completing these exercises, you should have a foundational understanding of how Docker containers and images work and how they can be managed from the command line.
