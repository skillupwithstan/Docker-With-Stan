# Docker with Stan 🐳

Welcome to the **Docker with Stan** repository! This repository is dedicated to learning, practicing, and mastering Docker concepts—from container basics to multi-container orchestration with Docker Compose.

---

## 📌 Introduction to Docker

**Docker** is an open-source platform that enables developers to build, package, package dependencies, and run applications inside lightweight, portable environments called **containers**.

### Why Docker?
* **Consistency:** "It works on my machine" is a thing of the past. Containers run identically in local development, testing, and production.
* **Isolation:** Applications run independently without interfering with system dependencies or other applications.
* **Lightweight:** Unlike traditional Virtual Machines (VMs), containers share the host operating system's kernel, making them start instantly and consume far fewer resources.

---

## 🏗️ Key Concepts

1. **Docker Engine:** The underlying client-server technology that creates and runs Docker containers.
2. **Dockerfile:** A text document containing instructions to build a Docker image.
3. **Docker Image:** A read-only template containing application code, libraries, runtime, environment variables, and configuration files.
4. **Docker Container:** A runnable, isolated instance of a Docker image.
5. **Docker Hub:** A public/private registry to share and download Docker images.
6. **Docker Compose:** A tool for defining and running multi-container Docker applications using a `docker-compose.yml` file.

---

## 🚀 Quick Start Guide

### 1. Check Docker Installation
```bash
docker --version

### **2. Run Your First Container**
```bash
docker run hello-world

### **3. Build an Image from Dockerfile**
docker build -t docker-with-stan:v1 .

### **4. Run the Container**
docker run -d -p 8080:80 --name my-stan-app docker-with-stan:v1

**🛠️ Essential Docker Commands**

**Command                                        Description**
docker images                       List all local Docker imagesdocker psList running containers
docker ps -a                        List all containers (running & stopped)
docker stop <container_id>          Stop a running container
docker rm <container_id>            Delete a container
docker rmi <image_id>               Delete a local image
docker logs <container_id>          View container logs
docker exec -it <container_id> bash Access container terminal

📂 Repository Structure

Docker-with-Stan/
├── README.md             # Overview and tutorial guide
├── Dockerfile            # Sample Dockerfile definition
├── docker-compose.yml    # Sample Docker Compose file
├── .gitignore            # Git ignore file for Docker artifacts
└── LICENSE               # MIT License
