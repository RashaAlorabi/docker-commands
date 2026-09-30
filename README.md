# 🐳 Docker Commands 

A simple and practical Docker reference containing the most commonly used commands and the basic Docker workflow.

---

# 📌 What is Docker?

Docker is a tool that helps you run applications easily on different machines without compatibility issues.

Normally, an application may require:

- A specific programming language version
- Libraries
- Dependencies
- Environment configuration

Docker packages the application and all its dependencies inside a **Container**, so the application can run consistently on different environments.

Basic idea:

```text
Application
+
Dependencies
+
Environment
=
Container
```

---

# 🔄 Docker Workflow

The basic Docker workflow is:

```text
Write Code
   ↓
Create Dockerfile
   ↓
Build Docker Image
   ↓
Run Container
   ↓
Tag Image
   ↓
Push Image to Docker Hub
   ↓
Pull Image on another machine
   ↓
Run Container
```

---

# 1️⃣ Create a Dockerfile

A `Dockerfile` contains the instructions Docker uses to build the application image.

Example for a Node.js application:

```dockerfile
FROM node:18

WORKDIR /app

COPY . .

RUN npm install

CMD ["node", "server.js"]
```

### Explanation

```dockerfile
FROM node:18
```

Uses Node.js version 18 as the base image.

---

```dockerfile
WORKDIR /app
```

Creates and sets `/app` as the working directory inside the container.

---

```dockerfile
COPY . .
```

Copies the application files from the current directory into the container.

---

```dockerfile
RUN npm install
```

Installs the application dependencies.

---

```dockerfile
CMD ["node", "server.js"]
```

Defines the command that runs when the container starts.

---

# 2️⃣ Build Docker Image

To build a Docker image from the Dockerfile:

```bash
docker build -t myapp .
```

### Explanation

```text
docker build
```

Builds a Docker image.

```text
-t myapp
```

Assigns the name `myapp` to the image.

```text
.
```

Tells Docker to use the Dockerfile in the current directory.

After running the command, Docker stores the image locally on your machine.

---

# 3️⃣ Run Docker Container

To create and run a container from the image:

```bash
docker run -d -p 3000:3000 myapp
```

### Explanation

```text
docker run
```

Creates and starts a container.

```text
-d
```

Runs the container in the background.

`d` means:

```text
detached mode
```

---

```text
-p 3000:3000
```

Maps the host machine port to the container port.

Format:

```text
HOST_PORT:CONTAINER_PORT
```

Example:

```text
3000:3000
```

means:

```text
Your Machine Port 3000
        ↓
Container Port 3000
```

---

```text
myapp
```

The Docker image name.

If the application is listening on port `3000`, you can open:

```text
http://localhost:3000
```

---

# 4️⃣ Login to Docker Hub

Before pushing an image to Docker Hub, login to your account:

```bash
docker login
```

Docker will ask for your Docker Hub credentials.

---

# 5️⃣ Tag Docker Image

Before pushing the image, tag it using your Docker Hub username.

```bash
docker tag myapp myusername/myapp
```

Example:

```bash
docker tag myapp rasha/myapp
```

General format:

```bash
docker tag LOCAL_IMAGE DOCKERHUB_USERNAME/IMAGE_NAME
```

---

# 6️⃣ Push Docker Image to Docker Hub

Upload the image to Docker Hub:

```bash
docker push myusername/myapp
```

Example:

```bash
docker push rasha/myapp
```

Now the image can be downloaded from another machine or server.

---

# 7️⃣ Pull Docker Image

To download an image from Docker Hub:

```bash
docker pull myusername/myapp
```

Example:

```bash
docker pull rasha/myapp
```

---

# 8️⃣ Run Image on Another Machine

After pulling the image:

```bash
docker run -d -p 3000:3000 myusername/myapp
```

Example:

```bash
docker run -d -p 3000:3000 rasha/myapp
```

---

# 🧠 Important Docker Concepts

## Dockerfile

A file containing instructions used to build a Docker image.

```text
Dockerfile
   ↓
docker build
   ↓
Docker Image
```

---

## Docker Image

A Docker Image is a packaged version of the application.

It contains:

- Application code
- Dependencies
- Runtime
- Required libraries
- Environment configuration

An image itself is not a running application.

---

## Docker Container

A Container is a running instance of a Docker Image.

```text
Image
  ↓
docker run
  ↓
Container
```

You can create multiple containers from the same image.

Example:

```text
myapp image
   ↓
Container 1
Container 2
Container 3
```

---

## Docker Hub

Docker Hub is a registry used to store and share Docker Images.

Example workflow:

```text
Local Machine
     ↓
docker push
     ↓
Docker Hub
     ↓
docker pull
     ↓
Another Machine / Server
```

---

# 📦 Image vs Container

| Docker Image | Docker Container |
|---|---|
| Package | Running application |
| Read-only template | Runtime instance |
| Created using `docker build` | Created using `docker run` |
| Can be stored in Docker Hub | Runs on a machine |
| One image can create many containers | Each container is an instance of an image |

---

# 🛠️ Common Docker Commands

## Check Docker Version

```bash
docker --version
```

Example output:

```text
Docker version 27.x.x
```

---

# 📋 List Running Containers

```bash
docker ps
```

Shows only currently running containers.

---

# 📋 List All Containers

```bash
docker ps -a
```

Shows:

- Running containers
- Stopped containers
- Exited containers

---

# 🖼️ List Docker Images

```bash
docker images
```

You can also use:

```bash
docker image ls
```

---

# ▶️ Start a Container

```bash
docker start CONTAINER_ID
```

Example:

```bash
docker start 12ab34cd56ef
```

You can also use the container name:

```bash
docker start my-container
```

---

# ⏹️ Stop a Container

```bash
docker stop CONTAINER_ID
```

Example:

```bash
docker stop 12ab34cd56ef
```

Or:

```bash
docker stop my-container
```

---

# 🔄 Restart a Container

```bash
docker restart CONTAINER_ID
```

Example:

```bash
docker restart my-container
```

---

# 🗑️ Remove a Container

The container should normally be stopped first.

```bash
docker rm CONTAINER_ID
```

Example:

```bash
docker rm my-container
```

---

# 🗑️ Force Remove a Running Container

```bash
docker rm -f CONTAINER_ID
```

Example:

```bash
docker rm -f my-container
```

---

# 🗑️ Remove Docker Image

```bash
docker rmi IMAGE_ID
```

Or:

```bash
docker rmi IMAGE_NAME
```

Example:

```bash
docker rmi myapp
```

---

# 📝 View Container Logs

```bash
docker logs CONTAINER_ID
```

Example:

```bash
docker logs my-container
```

---

# 📝 Follow Container Logs

To keep watching the logs in real time:

```bash
docker logs -f CONTAINER_ID
```

Example:

```bash
docker logs -f my-container
```

Press:

```text
Ctrl + C
```

to stop following the logs.

---

# 💻 Enter a Running Container

For containers that have `bash`:

```bash
docker exec -it CONTAINER_ID bash
```

Example:

```bash
docker exec -it my-container bash
```

For lightweight containers that do not contain `bash`, you may use:

```bash
docker exec -it my-container sh
```

---

# 🔍 Inspect a Container

```bash
docker inspect CONTAINER_ID
```

Example:

```bash
docker inspect my-container
```

This shows detailed information such as:

- IP address
- Network
- Environment variables
- Mounts
- Ports
- Container configuration

---

# 🏷️ Give a Container a Name

Instead of letting Docker generate a random name:

```bash
docker run --name my-container myapp
```

Example with ports:

```bash
docker run -d --name my-container -p 3000:3000 myapp
```

Now you can use:

```bash
docker stop my-container
```

instead of using the container ID.

---

# 🌐 Port Mapping

Syntax:

```bash
docker run -p HOST_PORT:CONTAINER_PORT IMAGE_NAME
```

Example:

```bash
docker run -p 8080:3000 myapp
```

This means:

```text
localhost:8080
      ↓
Container:3000
```

So you would open:

```text
http://localhost:8080
```

---

# 🌱 Environment Variables

Pass environment variables when starting the container:

```bash
docker run -e VARIABLE_NAME=value IMAGE_NAME
```

Example:

```bash
docker run -e NODE_ENV=production myapp
```

Multiple variables:

```bash
docker run \
  -e NODE_ENV=production \
  -e PORT=3000 \
  myapp
```

---

# 📄 Environment File

You can also load environment variables from a file.

Example `.env`:

```env
NODE_ENV=production
PORT=3000
DATABASE_HOST=localhost
```

Run:

```bash
docker run --env-file .env myapp
```

---

# 💾 Docker Volumes

Containers are temporary.

If a container is deleted, data stored inside it may also be deleted.

Docker Volumes allow data to persist.

Create a volume:

```bash
docker volume create my-volume
```

List volumes:

```bash
docker volume ls
```

Inspect a volume:

```bash
docker volume inspect my-volume
```

Remove a volume:

```bash
docker volume rm my-volume
```

---

# 💾 Run Container with Volume

Example:

```bash
docker run -v my-volume:/app/data myapp
```

Meaning:

```text
Docker Volume
my-volume
     ↓
Container
/app/data
```

---

# 📂 Bind Mount

A bind mount connects a folder from your machine directly to the container.

Example:

```bash
docker run -v $(pwd):/app myapp
```

Useful during development because code changes on your machine can appear inside the container.

---

# 🌐 Docker Networks

List networks:

```bash
docker network ls
```

Create a network:

```bash
docker network create my-network
```

Run container inside a network:

```bash
docker run --network my-network myapp
```

Remove network:

```bash
docker network rm my-network
```

Docker networks are especially useful when multiple containers need to communicate.

Example:

```text
Backend Container
       ↕
Docker Network
       ↕
Database Container
```

---

# 📊 Container Resource Usage

To see CPU and memory usage:

```bash
docker stats
```

Output includes:

```text
CPU
Memory
Network
Processes
```

---

# 📁 Copy Files Between Host and Container

Copy file from host to container:

```bash
docker cp file.txt CONTAINER_ID:/app/file.txt
```

Copy file from container to host:

```bash
docker cp CONTAINER_ID:/app/file.txt .
```

---

# 🧹 Docker Cleanup Commands

## Remove Stopped Containers

```bash
docker container prune
```

---

## Remove Unused Images

```bash
docker image prune
```

---

## Remove Unused Networks

```bash
docker network prune
```

---

## Remove Unused Volumes

```bash
docker volume prune
```

---

## Remove Unused Docker Resources

```bash
docker system prune
```

Be careful when using cleanup commands because Docker may remove unused resources.

---

# 🔎 Docker Disk Usage

Check how much space Docker is using:

```bash
docker system df
```

---

# ⚡ Build Without Cache

Sometimes Docker uses cached layers.

To force a fresh build:

```bash
docker build --no-cache -t myapp .
```

---

# 🏷️ Docker Image Versions

You can tag images with versions.

Example:

```bash
docker build -t myapp:1.0 .
```

Another version:

```bash
docker build -t myapp:2.0 .
```

List them:

```bash
docker images
```

Example:

```text
REPOSITORY   TAG
myapp        1.0
myapp        2.0
```

---

# 🚀 Push Image with Version

Tag:

```bash
docker tag myapp:1.0 myusername/myapp:1.0
```

Push:

```bash
docker push myusername/myapp:1.0
```

Pull:

```bash
docker pull myusername/myapp:1.0
```

Run:

```bash
docker run myusername/myapp:1.0
```

---

# 🔥 Complete Example

Assume we have:

```text
my-project/
│
├── Dockerfile
├── package.json
├── package-lock.json
└── server.js
```

Dockerfile:

```dockerfile
FROM node:18

WORKDIR /app

COPY . .

RUN npm install

CMD ["node", "server.js"]
```

---

## Step 1 — Build

```bash
docker build -t myapp .
```

---

## Step 2 — Check Image

```bash
docker images
```

---

## Step 3 — Run Container

```bash
docker run -d --name my-container -p 3000:3000 myapp
```

---

## Step 4 — Check Running Container

```bash
docker ps
```

---

## Step 5 — Check Logs

```bash
docker logs my-container
```

Or follow the logs:

```bash
docker logs -f my-container
```

---

## Step 6 — Open Application

```text
http://localhost:3000
```

---

## Step 7 — Stop Container

```bash
docker stop my-container
```

---

## Step 8 — Start Container Again

```bash
docker start my-container
```

---

## Step 9 — Remove Container

```bash
docker stop my-container

docker rm my-container
```

---

## Step 10 — Login to Docker Hub

```bash
docker login
```

---

## Step 11 — Tag Image

```bash
docker tag myapp myusername/myapp
```

---

## Step 12 — Push Image

```bash
docker push myusername/myapp
```

---

## Step 13 — Pull on Another Machine

```bash
docker pull myusername/myapp
```

---

## Step 14 — Run on Another Machine

```bash
docker run -d -p 3000:3000 myusername/myapp
```

---

# 📌 Quick Docker Cheat Sheet

| Task | Command |
|---|---|
| Check Docker version | `docker --version` |
| Build image | `docker build -t myapp .` |
| List images | `docker images` |
| Run container | `docker run myapp` |
| Run in background | `docker run -d myapp` |
| Map ports | `docker run -p 3000:3000 myapp` |
| Name container | `docker run --name my-container myapp` |
| Running containers | `docker ps` |
| All containers | `docker ps -a` |
| Stop container | `docker stop CONTAINER_ID` |
| Start container | `docker start CONTAINER_ID` |
| Restart container | `docker restart CONTAINER_ID` |
| Remove container | `docker rm CONTAINER_ID` |
| Remove image | `docker rmi IMAGE_ID` |
| View logs | `docker logs CONTAINER_ID` |
| Follow logs | `docker logs -f CONTAINER_ID` |
| Enter container | `docker exec -it CONTAINER_ID bash` |
| Inspect container | `docker inspect CONTAINER_ID` |
| Docker statistics | `docker stats` |
| Login to Docker Hub | `docker login` |
| Tag image | `docker tag myapp username/myapp` |
| Push image | `docker push username/myapp` |
| Pull image | `docker pull username/myapp` |
| List volumes | `docker volume ls` |
| List networks | `docker network ls` |
| Docker disk usage | `docker system df` |
| Cleanup unused resources | `docker system prune` |

---

# 🧠 Commands to Remember First

If you are starting with Docker, focus first on these commands:

```bash
docker build -t myapp .
```

```bash
docker run -d -p 3000:3000 myapp
```

```bash
docker ps
```

```bash
docker ps -a
```

```bash
docker logs my-container
```

```bash
docker stop my-container
```

```bash
docker start my-container
```

```bash
docker rm my-container
```

```bash
docker images
```

```bash
docker rmi myapp
```

```bash
docker login
```

```bash
docker tag myapp myusername/myapp
```

```bash
docker push myusername/myapp
```

```bash
docker pull myusername/myapp
```

---

# 🎯 Basic Docker Flow to Remember

```text
1. Write Application
        ↓
2. Create Dockerfile
        ↓
3. Build Image
        ↓
docker build -t myapp .
        ↓
4. Run Container
        ↓
docker run -d -p 3000:3000 myapp
        ↓
5. Test Application
        ↓
6. Login to Docker Hub
        ↓
docker login
        ↓
7. Tag Image
        ↓
docker tag myapp myusername/myapp
        ↓
8. Push Image
        ↓
docker push myusername/myapp
        ↓
9. Pull on Another Machine
        ↓
docker pull myusername/myapp
        ↓
10. Run
        ↓
docker run -d -p 3000:3000 myusername/myapp
```

---

# 📚 Repository Purpose

This repository is my personal Docker reference for reviewing:

- Docker fundamentals
- Docker Images
- Docker Containers
- Dockerfiles
- Port mapping
- Docker Hub
- Docker Volumes
- Docker Networks
- Container logs
- Container management
- Common Docker commands

It will be updated as I continue learning Docker and containerization.

---

# 🚀 Next Topics

Topics to add later:

- Docker Compose
- Multi-container applications
- Docker Compose with Database
- Docker networking in depth
- Docker volumes in depth
- Multi-stage builds
- Docker health checks
- Docker with FastAPI
- Docker with Laravel
- Docker with React / Next.js
- Docker with CI/CD
- Docker production best practices

---

# 🐳 Docker in One Sentence

> Docker packages an application and its dependencies into an isolated container so it can run consistently across different environments.
