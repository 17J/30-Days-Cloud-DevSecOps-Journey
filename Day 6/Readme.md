# Day 6 — Docker & Containerization

Modern software development has changed completely.

Applications are no longer deployed directly on servers like before.

Today, companies package applications into **containers** so they can run consistently anywhere:

- Developer laptop
- Cloud servers
- Kubernetes clusters
- CI/CD pipelines
- Edge infrastructure

## 🔗 Resources

- **GitHub Repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

- **Docker Command Sheet:**
  [https://docker-command.vercel.app/](https://docker-command.vercel.app/)

---

## 🧠 What is a Container?

A container is a lightweight isolated environment that includes:

- Application code
- Runtime
- Libraries
- Dependencies
- Environment variables
- System tools

Everything required to run the application is packaged together.

That means:

- Same behavior everywhere
- Faster deployments
- Portable infrastructure
- Easy scaling

---

## ⚔️ Containers vs Virtual Machines

This is one of the most important concepts in DevOps.

Both Containers and VMs isolate applications — but in very different ways.

---

## 🖥️ Virtual Machines (VMs)

A VM includes:

- Full Operating System
- Guest Kernel
- Hypervisor
- Application

### Problems with VMs

- High memory usage
- Slow startup time
- Large storage consumption
- Less efficient scaling

---

## 🐳 Containers

![Container ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/06gtggno8y76scmgmdzl.png)

Containers share the host OS kernel.

They only package:

- Application
- Dependencies
- Runtime

This makes them:

- Lightweight
- Fast
- Portable
- Efficient

---

## 📊 Containers vs VMs Comparison

| Feature        | Containers    | Virtual Machines |
| -------------- | ------------- | ---------------- |
| Startup Time   | Seconds       | Minutes          |
| Size           | MBs           | GBs              |
| Performance    | Near-native   | Heavy overhead   |
| Isolation      | Process-level | Full OS-level    |
| Resource Usage | Low           | High             |
| Portability    | Excellent     | Moderate         |

---

## 📦 What is Docker?

Docker is a platform used to:

- Build applications
- Package applications
- Ship applications
- Run applications in isolated environments called **containers**

Docker ensures:

> “It works the same everywhere.”

This solves the classic developer problem:

> “It works on my machine.”

---

## 🏗️ Docker Architecture

Docker mainly consists of 3 components:

![Docker Registry](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4sv8qwyu0qp1nzy80id3.png)

---

## 1️⃣ Docker Client

The CLI where commands are executed.

Example:

```bash
docker build
docker run
docker ps
```

---

## 2️⃣ Docker Daemon

The background service (`dockerd`) responsible for managing:

- Containers
- Images
- Networks
- Volumes

---

## 3️⃣ Docker Registry

A place where Docker images are stored.

Popular registries include:

- Docker Hub
- GitHub Container Registry
- AWS ECR
- Google Artifact Registry

---

## 🧱 What is a Docker Image?

A Docker Image is a:

> Read-only blueprint used to create containers.

It contains:

- Application code
- Dependencies
- Runtime
- Configuration
- Libraries

---

## 🧠 Simple Analogy

| Concept          | Real World Example |
| ---------------- | ------------------ |
| Docker Image     | Recipe             |
| Docker Container | Cooked Food        |

The image is the template.

The container is the running instance.

---

## 🔍 Popular Docker Images

Some commonly used images:

```bash
nginx
ubuntu
node
python
mysql
redis
postgres
```

Pull an image:

```bash
docker pull nginx
```

Run a container:

```bash
docker run nginx
```

---

## 📝 What is a Dockerfile?

A `Dockerfile` is a text file containing instructions used to build a Docker image.

This is where containerization begins.

---

## 🧱 Example Dockerfile

```Dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
```

---

## 🔬 Dockerfile Explained

---

## `FROM`

Defines the base image.

```Dockerfile
FROM node:20-alpine
```

---

## `WORKDIR`

Sets the working directory inside the container.

```Dockerfile
WORKDIR /app
```

---

## `COPY`

Copies files from local system into the container.

```Dockerfile
COPY . .
```

---

## `RUN`

Executes commands during image build.

```Dockerfile
RUN npm install
```

---

## `EXPOSE`

Documents which port the application uses.

```Dockerfile
EXPOSE 3000
```

---

## `CMD`

Defines the default startup command.

```Dockerfile
CMD ["node", "app.js"]
```

---

## 🚀 Building Docker Images

Build image:

```bash
docker build -t myapp .
```

### Explanation

| Part           | Meaning            |
| -------------- | ------------------ |
| `docker build` | Build Docker image |
| `-t`           | Tag image          |
| `myapp`        | Image name         |
| `.`            | Current directory  |

---

## ▶️ Running Containers

Run container:

```bash
docker run -p 3000:3000 myapp
```

---

## 🔍 Understanding Port Mapping

```bash
-p 3000:3000
```

Means:

| Host Machine | Container |
| ------------ | --------- |
| 3000         | 3000      |

Access app on:

```text
http://localhost:3000
```

---

## 📂 Containerizing a Node.js Application

---

## 📁 Project Structure

```text
project/
│
├── app.js
├── package.json
└── Dockerfile
```

---

## 📄 app.js

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Docker is working!");
});

app.listen(3000);
```

---

## 📄 package.json

```json
{
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## 📄 Dockerfile

```Dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
```

---

## 🏗️ Build the Image

```bash
docker build -t node-docker-demo .
```

---

## ▶️ Run the Container

```bash
docker run -p 3000:3000 node-docker-demo
```

Now open:

```text
http://localhost:3000
```

You should see:

```text
Docker is working!
```

---

## 📦 Docker Image Layers

Docker images are layered.

Each instruction creates a new layer.

Example:

```Dockerfile
FROM ubuntu:24.04
RUN apt update
RUN apt install nginx
COPY . .
```

Each line becomes a cached layer.

This makes builds:

- Faster
- Efficient
- Reusable

---

## ⚡ Docker Caching

Docker rebuilds only changed layers.

That’s why Dockerfiles should be optimized carefully.

---

## ✅ Cache-Friendly Pattern

```Dockerfile
COPY package.json .
RUN npm install

COPY . .
```

### Why?

Dependencies are cached separately.

If only source code changes, Docker skips reinstalling packages.

---

## ❌ Bad Pattern

```Dockerfile
COPY . .
RUN npm install
```

Any file change invalidates cache.

Build becomes slower.

---

## 📁 Docker Volumes

Containers are ephemeral.

Data disappears after container removal.

Volumes solve this problem.

---

## Create Volume

```bash
docker volume create mydata
```

Use volume:

```bash
docker run -v mydata:/data nginx
```

---

## 🌐 Docker Networking

Containers communicate using Docker networks.

Create network:

```bash
docker network create mynetwork
```

Run containers inside same network.

Useful for:

- APIs
- Databases
- Microservices
- Internal communication

---

### 🧹 Essential Docker Commands

---

## 📦 Images

```bash
docker images
docker pull nginx
docker build -t myapp .
docker push username/myapp:v1
docker rmi image_id
```

---

## 🐳 Containers

```bash
docker ps
docker ps -a
docker stop container_id
docker rm container_id
docker logs -f container_id
docker exec -it container_id sh
```

---

## 🧽 Cleanup

```bash
docker system prune
docker volume prune
```

---

## 🔥 Why Docker Became So Popular

Docker transformed software deployment because it provides:

- Environment consistency
- Faster deployments
- Infrastructure portability
- Easy CI/CD integration
- Better scalability
- Microservices support
- Cloud-native compatibility

---

## ☁️ Docker in Modern DevOps

Docker is now everywhere.

| Technology      | Docker Usage       |
| --------------- | ------------------ |
| Kubernetes      | Runs containers    |
| CI/CD           | Build & deploy     |
| Cloud Platforms | Container hosting  |
| DevSecOps       | Isolated workloads |
| Microservices   | Service packaging  |

---

## 🔐 Docker Security Basics

Important security practices:

- Use minimal base images
- Avoid running as root
- Scan images regularly
- Keep dependencies updated
- Use signed images
- Remove unused containers/images

---

## 🏭 Real-World Docker & K8s Workflow

![Docker WorkFlow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zq6l2q7qqosf94imd00t.png)

---

## 🚀 Docker Best Practices

---

## ✅ Use Smaller Images

Prefer:

```Dockerfile
FROM node:20-alpine
```

Instead of huge base images.

Smaller images:

- Pull faster
- Reduce attack surface
- Save storage

---

## ✅ Always Use `.dockerignore`

Example:

```text
node_modules
.git
.env
*.log
```

This reduces build size and prevents accidental secret leaks.

---

## ✅ Multi-Stage Builds

Use one stage for building.

Use another minimal stage for production.

---

## ✅ Tag Images Properly

Bad:

```text
latest
```

Good:

```text
v1.0.2
```

---

## ✅ Don’t Run Containers as Root

Example:

```Dockerfile
RUN adduser -S appuser
USER appuser
```

---

## 🎭 Multi-Stage Build Example

```Dockerfile
# Stage 1 — Build
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# Stage 2 — Production
FROM node:20-alpine

WORKDIR /app

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

---

## 📚 Final Thoughts

Docker completely changed how modern applications are built and deployed.

Understanding:

- Containers
- Docker Images
- Dockerfiles
- Layers
- Volumes
- Networking

is now a fundamental engineering skill.
Because modern infrastructure runs on containers.

most popular alternatives to Docker for containerization and container runtime workflows:

- Podman
- containerd
- CRI-O

---

## 🎯 Quick Recap

| Concept      | Meaning                 |
| ------------ | ----------------------- |
| Container    | Isolated runtime        |
| Docker Image | Blueprint/template      |
| Dockerfile   | Build instructions      |
| Container    | Running image instance  |
| Volume       | Persistent storage      |
| Network      | Container communication |
