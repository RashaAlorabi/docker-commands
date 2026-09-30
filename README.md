# 🐳 Docker Commands 

A practical Docker reference for learning, reviewing, and using the most important Docker commands.

This repository includes:

- Docker basics
- Dockerfile
- Images
- Containers
- Ports
- Volumes
- Networks
- Logs and debugging
- Docker Hub
- Docker Compose
- Cleanup commands
- Useful production commands

---

# 📌 What is Docker?

Docker is a tool that helps package an application with its dependencies so it can run consistently across different environments.

Instead of installing everything manually on every machine, Docker packages:

```text
Application
+
Dependencies
+
Runtime
+
Libraries
+
Configuration
=
Docker Image
```

Then the image can be started as a:

```text
Container
```

---

# 🔄 Docker Basic Workflow

```text
Write Code
   ↓
Create Dockerfile
   ↓
Build Docker Image
   ↓
Run Container
   ↓
Test Application
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

# 🧠 Core Docker Concepts

## Dockerfile

A `Dockerfile` contains instructions Docker uses to build an image.

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

It may include:

- Application code
- Runtime
- Dependencies
- Libraries
- Configuration

An image is not running by itself.

---

## Docker Container

A Container is a running instance of an Image.

```text
Image
  ↓
docker run
  ↓
Container
```

One image can create multiple containers:

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

```text
Local Machine
     ↓
docker push
     ↓
Docker Hub
     ↓
docker pull
     ↓
Another Machine
```

---

# 🐳 Dockerfile Example

Example for a Node.js application:

```dockerfile
FROM node:18

WORKDIR /app

COPY . .

RUN npm install

CMD ["node", "server.js"]
```

---

# 📘 Important Dockerfile Instructions

## FROM

Defines the base image.

```dockerfile
FROM node:18
```

Example:

```dockerfile
FROM python:3.12
```

---

## WORKDIR

Sets the working directory inside the container.

```dockerfile
WORKDIR /app
```

---

## COPY

Copies files into the image.

```dockerfile
COPY . .
```

Better example:

```dockerfile
COPY package*.json ./
RUN npm install
COPY . .
```

---

## RUN

Runs a command while building the image.

```dockerfile
RUN npm install
```

Another example:

```dockerfile
RUN pip install -r requirements.txt
```

---

## CMD

Defines the default command when the container starts.

```dockerfile
CMD ["node", "server.js"]
```

---

## ENTRYPOINT

Defines the main executable.

```dockerfile
ENTRYPOINT ["node"]
```

Can be combined with:

```dockerfile
CMD ["server.js"]
```

---

## ENV

Defines environment variables inside the image.

```dockerfile
ENV NODE_ENV=production
```

---

## ARG

Defines build-time variables.

```dockerfile
ARG APP_VERSION=1.0
```

Build with:

```bash
docker build --build-arg APP_VERSION=2.0 -t myapp .
```

---

## EXPOSE

Documents the port used by the application.

```dockerfile
EXPOSE 3000
```

Important:

`EXPOSE` does not automatically publish the port.

You still need:

```bash
docker run -p 3000:3000 myapp
```

---

## USER

Runs the application as a specific user.

```dockerfile
USER node
```

This can improve security.

---

## HEALTHCHECK

Checks whether the application is healthy.

Example:

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:3000 || exit 1
```

---

# 🏗️ Build Docker Image

Build an image:

```bash
docker build -t myapp .
```

Explanation:

```text
docker build
```

Build the image.

```text
-t myapp
```

Set image name/tag.

```text
.
```

Use the current directory as build context.

---

# 🏷️ Build Image with Version

```bash
docker build -t myapp:1.0 .
```

Another version:

```bash
docker build -t myapp:2.0 .
```

---

# ♻️ Build Without Cache

```bash
docker build --no-cache -t myapp .
```

Useful when Docker cache causes issues.

---

# 🖼️ List Docker Images

```bash
docker images
```

Or:

```bash
docker image ls
```

---

# 🔎 Inspect Image

```bash
docker image inspect myapp
```

---

# 📚 Image History

See image layers:

```bash
docker history myapp
```

---

# 🗑️ Remove Image

```bash
docker rmi myapp
```

Or:

```bash
docker image rm myapp
```

Force removal:

```bash
docker rmi -f myapp
```

---

# ▶️ Run Docker Container

```bash
docker run -d -p 3000:3000 myapp
```

Explanation:

```text
-d
```

Run in detached mode.

```text
-p 3000:3000
```

Map ports.

Format:

```text
HOST_PORT:CONTAINER_PORT
```

---

# 🏷️ Run Container with Name

```bash
docker run -d --name my-container -p 3000:3000 myapp
```

Now you can use:

```bash
docker stop my-container
```

instead of the container ID.

---

# 🧪 Run and Automatically Remove Container

```bash
docker run --rm myapp
```

Useful for temporary containers.

---

# 💻 Run Interactive Container

```bash
docker run -it myapp sh
```

If bash is available:

```bash
docker run -it myapp bash
```

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

Then access:

```text
http://localhost:8080
```

---

# 📋 List Running Containers

```bash
docker ps
```

---

# 📋 List All Containers

```bash
docker ps -a
```

---

# ▶️ Start Container

```bash
docker start my-container
```

---

# ⏹️ Stop Container

```bash
docker stop my-container
```

`docker stop` allows the application some time to shut down gracefully.

---

# 💥 Kill Container Immediately

```bash
docker kill my-container
```

Use when the container does not stop normally.

---

# 🔄 Restart Container

```bash
docker restart my-container
```

---

# ⏸️ Pause Container

```bash
docker pause my-container
```

Resume:

```bash
docker unpause my-container
```

---

# 🗑️ Remove Container

```bash
docker rm my-container
```

Force remove:

```bash
docker rm -f my-container
```

---

# 🏷️ Rename Container

```bash
docker rename old-name new-name
```

---

# 🔍 Inspect Container

```bash
docker inspect my-container
```

Useful for:

- IP address
- Ports
- Environment variables
- Volumes
- Networks
- Configuration

---

# 🚪 Show Container Ports

```bash
docker port my-container
```

---

# ⚙️ Show Running Processes

```bash
docker top my-container
```

---

# 🧩 Container File Changes

```bash
docker diff my-container
```

Shows files changed inside the container.

---

# 📝 View Logs

```bash
docker logs my-container
```

---

# 📝 Follow Logs

```bash
docker logs -f my-container
```

---

# 📝 Last 100 Log Lines

```bash
docker logs --tail 100 my-container
```

---

# 📝 Logs Since a Specific Time

```bash
docker logs --since 10m my-container
```

---

# 💻 Enter Running Container

```bash
docker exec -it my-container bash
```

If bash does not exist:

```bash
docker exec -it my-container sh
```

---

# 🔗 Attach to Main Process

```bash
docker attach my-container
```

Be careful because this connects directly to the main process.

---

# ⏳ Wait for Container to Exit

```bash
docker wait my-container
```

Returns the exit code.

---

# 📊 Monitor Resource Usage

```bash
docker stats
```

Shows:

- CPU
- Memory
- Network
- Processes

---

# 📄 Container Metadata

```bash
docker container inspect my-container
```

---

# 🌱 Environment Variables

Pass one variable:

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

Example `.env`:

```env
NODE_ENV=production
PORT=3000
DATABASE_HOST=db
```

Run:

```bash
docker run --env-file .env myapp
```

---

# 💾 Docker Volumes

Containers can be deleted.

Important data should not live only inside a container.

Docker Volumes persist data.

---

# ➕ Create Volume

```bash
docker volume create my-volume
```

---

# 📋 List Volumes

```bash
docker volume ls
```

---

# 🔎 Inspect Volume

```bash
docker volume inspect my-volume
```

---

# ▶️ Run Container with Volume

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

# 🗑️ Remove Volume

```bash
docker volume rm my-volume
```

---

# 🧹 Remove Unused Volumes

```bash
docker volume prune
```

---

# 📂 Bind Mount

A bind mount connects a local folder directly to the container.

```bash
docker run -v $(pwd):/app myapp
```

Useful during development.

---

# 🌐 Docker Networks

Networks allow containers to communicate.

Example:

```text
Backend Container
       ↕
Docker Network
       ↕
Database Container
```

---

# ➕ Create Network

```bash
docker network create my-network
```

---

# 📋 List Networks

```bash
docker network ls
```

---

# 🔎 Inspect Network

```bash
docker network inspect my-network
```

---

# ▶️ Run Container in Network

```bash
docker run --network my-network myapp
```

---

# 🔗 Connect Container to Network

```bash
docker network connect my-network my-container
```

---

# 🔌 Disconnect Container from Network

```bash
docker network disconnect my-network my-container
```

---

# 🗑️ Remove Network

```bash
docker network rm my-network
```

---

# 🧹 Remove Unused Networks

```bash
docker network prune
```

---

# 📁 Copy Files

Copy from host to container:

```bash
docker cp file.txt my-container:/app/file.txt
```

Copy from container to host:

```bash
docker cp my-container:/app/file.txt .
```

---

# 🔐 Docker Hub Login

```bash
docker login
```

---

# 🏷️ Tag Image

```bash
docker tag myapp myusername/myapp
```

With version:

```bash
docker tag myapp:1.0 myusername/myapp:1.0
```

---

# ⬆️ Push Image

```bash
docker push myusername/myapp
```

Version:

```bash
docker push myusername/myapp:1.0
```

---

# ⬇️ Pull Image

```bash
docker pull myusername/myapp
```

Version:

```bash
docker pull myusername/myapp:1.0
```

---

# 🌍 Run Image on Another Machine

```bash
docker pull myusername/myapp
```

Then:

```bash
docker run -d -p 3000:3000 myusername/myapp
```

---

# 💾 Save Docker Image to File

```bash
docker save -o myapp.tar myapp
```

---

# 📥 Load Docker Image from File

```bash
docker load -i myapp.tar
```

---

# 📦 Export Container Filesystem

```bash
docker export my-container > container.tar
```

---

# 📥 Import Filesystem as Image

```bash
docker import container.tar my-new-image
```

---

# 🧬 Commit Container to Image

```bash
docker commit my-container my-new-image
```

This exists, but it is usually better to define changes in a Dockerfile.

---

# ℹ️ Docker Information

```bash
docker info
```

Shows:

- Docker Engine information
- Storage driver
- Number of images
- Number of containers
- Runtime
- Networks

---

# 📅 Docker Events

```bash
docker events
```

Shows Docker events in real time.

---

# 🌍 Docker Contexts

List contexts:

```bash
docker context ls
```

Switch context:

```bash
docker context use CONTEXT_NAME
```

---

# 🧹 Docker Cleanup

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

## Remove Build Cache

```bash
docker builder prune
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

---

## More Aggressive Cleanup

```bash
docker system prune -a
```

Be careful.

This removes unused images, not only dangling images.

---

# 💽 Docker Disk Usage

```bash
docker system df
```

---

# 🧰 Docker Compose

Docker Compose is used to manage multiple containers together.

Example:

```text
Backend
Database
Redis
Kafka
```

Instead of starting every container manually, Docker Compose allows you to manage them from one configuration file.

---

# 📄 Basic compose.yaml Example

```yaml
services:

  backend:
    build: .
    ports:
      - "3000:3000"

  database:
    image: postgres:16
    environment:
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb
```

---

# ▶️ Start Docker Compose Services

```bash
docker compose up
```

---

# ▶️ Start in Background

```bash
docker compose up -d
```

---

# 🏗️ Build Services

```bash
docker compose build
```

---

# 🔄 Build and Start

```bash
docker compose up --build
```

---

# ⏹️ Stop and Remove Services

```bash
docker compose down
```

---

# 📋 List Compose Services

```bash
docker compose ps
```

---

# 📝 View Compose Logs

```bash
docker compose logs
```

---

# 📝 Follow Compose Logs

```bash
docker compose logs -f
```

---

# 📝 Logs for Specific Service

```bash
docker compose logs backend
```

---

# 💻 Enter Compose Service

```bash
docker compose exec backend bash
```

Or:

```bash
docker compose exec backend sh
```

---

# 🔄 Restart Services

```bash
docker compose restart
```

Specific service:

```bash
docker compose restart backend
```

---

# ⬇️ Pull Compose Images

```bash
docker compose pull
```

---

# ⏹️ Stop Without Removing

```bash
docker compose stop
```

---

# ▶️ Start Existing Services

```bash
docker compose start
```

---

# 🗑️ Remove Stopped Compose Containers

```bash
docker compose rm
```

---

# 📈 Scale Service

Example:

```bash
docker compose up -d --scale backend=3
```

Creates:

```text
backend-1
backend-2
backend-3
```

---

# 🐳 Complete Docker Workflow Example

Project:

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

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

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
docker run -d \
  --name my-container \
  -p 3000:3000 \
  myapp
```

---

## Step 4 — Check Container

```bash
docker ps
```

---

## Step 5 — View Logs

```bash
docker logs -f my-container
```

---

## Step 6 — Open Application

```text
http://localhost:3000
```

---

## Step 7 — Enter Container

```bash
docker exec -it my-container sh
```

---

## Step 8 — Stop Container

```bash
docker stop my-container
```

---

## Step 9 — Start Again

```bash
docker start my-container
```

---

## Step 10 — Remove Container

```bash
docker stop my-container
docker rm my-container
```

---

## Step 11 — Login to Docker Hub

```bash
docker login
```

---

## Step 12 — Tag Image

```bash
docker tag myapp myusername/myapp
```

---

## Step 13 — Push Image

```bash
docker push myusername/myapp
```

---

## Step 14 — Pull on Another Machine

```bash
docker pull myusername/myapp
```

---

## Step 15 — Run on Another Machine

```bash
docker run -d \
  -p 3000:3000 \
  myusername/myapp
```

---

# 🧰 Complete Docker Compose Example

```yaml
services:

  backend:
    build: .
    container_name: backend
    ports:
      - "3000:3000"
    environment:
      DATABASE_HOST: database
    depends_on:
      - database
    networks:
      - app-network

  database:
    image: postgres:16
    container_name: database
    environment:
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network

volumes:
  postgres-data:

networks:
  app-network:
```

Start:

```bash
docker compose up -d
```

Check:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs -f
```

Stop:

```bash
docker compose down
```

---

# 📌 Quick Docker Cheat Sheet

| Task | Command |
|---|---|
| Docker version | `docker --version` |
| Docker info | `docker info` |
| Build image | `docker build -t myapp .` |
| Build without cache | `docker build --no-cache -t myapp .` |
| List images | `docker images` |
| Inspect image | `docker image inspect myapp` |
| Image history | `docker history myapp` |
| Remove image | `docker rmi myapp` |
| Run container | `docker run myapp` |
| Run background | `docker run -d myapp` |
| Run interactive | `docker run -it myapp sh` |
| Auto remove | `docker run --rm myapp` |
| Map ports | `docker run -p 3000:3000 myapp` |
| Name container | `docker run --name my-container myapp` |
| Running containers | `docker ps` |
| All containers | `docker ps -a` |
| Start container | `docker start my-container` |
| Stop container | `docker stop my-container` |
| Kill container | `docker kill my-container` |
| Restart container | `docker restart my-container` |
| Pause container | `docker pause my-container` |
| Resume container | `docker unpause my-container` |
| Remove container | `docker rm my-container` |
| View logs | `docker logs my-container` |
| Follow logs | `docker logs -f my-container` |
| Enter container | `docker exec -it my-container sh` |
| Inspect container | `docker inspect my-container` |
| Container ports | `docker port my-container` |
| Resource usage | `docker stats` |
| Create volume | `docker volume create my-volume` |
| List volumes | `docker volume ls` |
| Inspect volume | `docker volume inspect my-volume` |
| Create network | `docker network create my-network` |
| List networks | `docker network ls` |
| Inspect network | `docker network inspect my-network` |
| Docker Hub login | `docker login` |
| Tag image | `docker tag myapp username/myapp` |
| Push image | `docker push username/myapp` |
| Pull image | `docker pull username/myapp` |
| Docker disk usage | `docker system df` |
| Cleanup | `docker system prune` |

---

# 📌 Quick Docker Compose Cheat Sheet

| Task | Command |
|---|---|
| Start services | `docker compose up` |
| Start background | `docker compose up -d` |
| Build services | `docker compose build` |
| Build and start | `docker compose up --build` |
| List services | `docker compose ps` |
| Logs | `docker compose logs` |
| Follow logs | `docker compose logs -f` |
| Stop services | `docker compose stop` |
| Start existing services | `docker compose start` |
| Restart services | `docker compose restart` |
| Enter service | `docker compose exec SERVICE sh` |
| Pull images | `docker compose pull` |
| Stop and remove | `docker compose down` |
| Remove stopped containers | `docker compose rm` |

---

# 🎯 Commands to Learn First

If you are still learning Docker, focus on these first:

```bash
docker build -t myapp .
```

```bash
docker run -d --name my-container -p 3000:3000 myapp
```

```bash
docker ps
```

```bash
docker ps -a
```

```bash
docker images
```

```bash
docker logs -f my-container
```

```bash
docker exec -it my-container sh
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

And for Docker Compose:

```bash
docker compose up -d
```

```bash
docker compose ps
```

```bash
docker compose logs -f
```

```bash
docker compose down
```

---

# 🧠 Docker Debugging Flow

When an application does not work:

```text
Container not running?
        ↓
docker ps -a
        ↓
Check logs
        ↓
docker logs CONTAINER
        ↓
Check configuration
        ↓
docker inspect CONTAINER
        ↓
Enter container
        ↓
docker exec -it CONTAINER sh
        ↓
Check ports
        ↓
docker port CONTAINER
        ↓
Check resources
        ↓
docker stats
```

---

# 🔐 Basic Best Practices

## Use Image Versions

Prefer:

```dockerfile
FROM node:18
```

instead of an uncontrolled image version.

---

## Avoid Running as Root

Use:

```dockerfile
USER node
```

when possible.

---

## Keep Images Small

Avoid unnecessary dependencies.

---

## Use `.dockerignore`

Example:

```text
node_modules
.git
.env
*.log
```

This prevents unnecessary files from being copied into the image.

---

## Do Not Put Secrets in Dockerfile

Avoid:

```dockerfile
ENV DATABASE_PASSWORD=mysecretpassword
```

Prefer environment variables or secret management.

---

## Use Multi-Stage Builds When Needed

Example:

```dockerfile
FROM node:18 AS builder

WORKDIR /app

COPY . .

RUN npm install
RUN npm run build

FROM node:18-alpine

WORKDIR /app

COPY --from=builder /app/dist ./dist

CMD ["node", "dist/server.js"]
```

---

# 🏗️ Docker in System Architecture

Docker allows the same application image to create multiple containers.

Example:

```text
             Load Balancer
                  |
       -----------------------
       |          |          |
   Container   Container   Container
       1           2          3
       \           |         /
              Database
```

This is commonly used with horizontal scaling.

---

# 🧩 Docker vs Image vs Container

```text
Dockerfile
   ↓
Build
   ↓
Image
   ↓
Run
   ↓
Container
```

Simple definition:

```text
Dockerfile = Instructions
Image      = Package
Container  = Running Instance
```

---

# 🚀 Learning Roadmap

After mastering these commands:

```text
Docker Fundamentals
       ↓
Dockerfile
       ↓
Images & Containers
       ↓
Volumes
       ↓
Networks
       ↓
Docker Compose
       ↓
Multi-Stage Builds
       ↓
Health Checks
       ↓
Docker Security
       ↓
CI/CD
       ↓
Container Orchestration
       ↓
Kubernetes
```

---

# 📚 Repository Purpose

This repository is my personal Docker reference for reviewing:

- Docker fundamentals
- Docker Images
- Docker Containers
- Dockerfiles
- Docker Hub
- Port mapping
- Environment variables
- Volumes
- Networks
- Logs
- Debugging
- Docker Compose
- Docker cleanup
- Docker best practices

It will be updated as I continue learning Docker, containerization, CI/CD, and software architecture.

---

# 🐳 Docker in One Sentence

> Docker packages an application and its dependencies into an isolated container so it can run consistently across different environments.
