# 🛠️ Essential Docker Commands Cheat Sheet

![Docker](https://img.shields.io/badge/Docker-Cheat--Sheet-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Terminal](https://img.shields.io/badge/CLI-Reference-black?style=for-the-badge&logo=gnu-bash)

A comprehensive guide to essential Docker commands categorized by lifecycle and functionality.

---

## 📑 Quick Navigation
- [1. Image Management](#1-image-management-)
- [2. Container Lifecycle](#2-container-lifecycle-)
- [3. Container Inspection & Logs](#3-container-inspection--logs-)
- [4. Docker Networking](#4-docker-networking-)
- [5. Volume & Data Management](#5-volume--data-management-)
- [6. Docker Compose Commands](#6-docker-compose-commands-)
- [7. System Maintenance & Cleanup](#7-system-maintenance--cleanup-)

---

## 1. Image Management 🖼️

| Command | Action | Example |
| :--- | :--- | :--- |
| `docker build` | Build an image from a `Dockerfile` | `docker build -t myapp:1.0 .` |
| `docker pull` | Download an image from Docker Hub | `docker pull nginx:latest` |
| `docker images` | List locally available images | `docker images` |
| `docker rmi` | Remove one or more images | `docker rmi myapp:1.0` |
| `docker tag` | Create a tag pointing to a target image | `docker tag myapp:1.0 username/myapp:v1` |
| `docker push` | Upload an image to a registry | `docker push username/myapp:v1` |

> [!TIP]
> **Example: Building & Tagging an Image**
> ```bash
> # Build image with tag 'my-web-app:v1' from current directory
> docker build -t my-web-app:v1 .
> ```

---

## 2. Container Lifecycle 📦

| Command | Action | Example |
| :--- | :--- | :--- |
| `docker run` | Create and start a container | `docker run -d -p 8080:80 --name my-web nginx` |
| `docker ps` | List running containers | `docker ps` |
| `docker ps -a` | List all containers (running & stopped) | `docker ps -a` |
| `docker start` | Start a stopped container | `docker start my-web` |
| `docker stop` | Gracefully stop a running container | `docker stop my-web` |
| `docker restart` | Restart a running/stopped container | `docker restart my-web` |
| `docker rm` | Remove a stopped container | `docker rm my-web` |
| `docker rm -f` | Force remove a running container | `docker rm -f my-web` |

> [!NOTE]
> **Example: Running an Interactive Container**
> ```bash
> # Run Ubuntu interactively with bash prompt and clean up on exit (--rm)
> docker run -it --rm ubuntu bash
> ```

---

## 3. Container Inspection & Logs 🔍

| Command | Action | Example |
| :--- | :--- | :--- |
| `docker logs` | Fetch logs of a container | `docker logs -f my-web` |
| `docker exec` | Execute a command inside a running container | `docker exec -it my-web bash` |
| `docker inspect` | Return detailed low-level JSON info | `docker inspect my-web` |
| `docker stats` | Live stream container resource usage | `docker stats` |
| `docker top` | Display running processes of a container | `docker top my-web` |

> [!TIP]
> **Example: Streaming Container Logs**
> ```bash
> # Follow logs live with timestamp
> docker logs -f --tail 100 my-web
> ```

---

## 4. Docker Networking 🌐

| Command | Action | Example |
| :--- | :--- | :--- |
| `docker network ls` | List all Docker networks | `docker network ls` |
| `docker network create` | Create a custom network | `docker network create app-net` |
| `docker network connect` | Connect a container to a network | `docker network connect app-net my-web` |
| `docker network disconnect`| Disconnect container from network | `docker network disconnect app-net my-web` |
| `docker network inspect` | Inspect network details | `docker network inspect app-net` |

---

## 5. Volume & Data Management 💾

| Command | Action | Example |
| :--- | :--- | :--- |
| `docker volume ls` | List all volumes | `docker volume ls` |
| `docker volume create` | Create a managed volume | `docker volume create db-data` |
| `docker volume inspect` | Inspect volume details | `docker volume inspect db-data` |
| `docker volume rm` | Remove a volume | `docker volume rm db-data` |

> [!NOTE]
> **Example: Running Postgres with persistent storage**
> ```bash
> docker run -d \
>   --name postgres-db \
>   -v db-data:/var/lib/postgresql/data \
>   -e POSTGRES_PASSWORD=secret \
>   postgres:15-alpine
> ```

---

## 6. Docker Compose Commands 🐙

| Command | Action |
| :--- | :--- |
| `docker compose up -d` | Start services in background |
| `docker compose down` | Stop and remove containers, networks, and volumes |
| `docker compose ps` | List containers managed by Compose |
| `docker compose logs -f` | Follow logs of all composed services |
| `docker compose build` | Rebuild service images defined in Compose file |

---

## 7. System Maintenance & Cleanup 🧹

> [!WARNING]
> Use cleanup commands with care, as they remove stopped containers or unused images.

| Command | Action |
| :--- | :--- |
| `docker system df` | Show Docker disk usage |
| `docker container prune` | Remove all stopped containers |
| `docker image prune -a` | Remove all unused images |
| `docker system prune` | Remove all unused containers, networks, images, and build cache |
| `docker system prune -a --volumes` | Deep clean everything not currently in use |
