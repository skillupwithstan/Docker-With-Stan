# ⏳ History and Evolution of Docker

![Evolution](https://img.shields.io/badge/Evolution-Timeline-2496ED?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-History-blue?style=for-the-badge&logo=docker&logoColor=white)

Docker did not invent container technology. Linux features like `chroot`, namespaces, and cgroups existed long before Docker. However, Docker revolutionized the industry by making containers **accessible, portable, and easy to use**.

Here is the evolutionary journey of how Docker's architecture matured from a simple wrapper to a modular, standardized platform.

---
## 📺 Watch the Video

Click the image below to watch the full video on the History and Evolution of Docker!

[![History and Evolution of Docker](https://img.youtube.com/vi/fd_2vIucrMo/maxresdefault.jpg)](https://youtu.be/fd_2vIucrMo)

---

## 🗺️ The Evolutionary Timeline

```text
 2013        2014             2015             2016+             Today
  │           │                │                │                  │
 [LXC] ──> [libcontainer] ──> [runc & OCI] ──> [containerd] ──> [VMMs & Sandboxing]


1️⃣ The Beginning: LXC (Early Docker)
When Docker was first released in 2013, it did not have its own container runtime. Instead, it acted as a user-friendly wrapper around LXC (Linux Containers).

How it worked: Docker used LXC as the underlying execution environment to interface with the Linux kernel's Namespaces and Control Groups (cgroups).

The Problem: LXC was tied heavily to Linux-specific configurations. As Docker grew rapidly, relying on a third-party tool that varied across different Linux distributions became a bottleneck for stability and cross-platform compatibility.

2️⃣ The Breakaway: libcontainer (2014 - v0.9.0)
To gain control over its own destiny, Docker dropped LXC as the default execution environment in Docker version 0.9.0 (released in early 2014) and introduced its own native tool: libcontainer.

What it was: Written entirely in Go, libcontainer allowed Docker to interact directly with the Linux kernel without needing LXC or any other intermediate user-space tools.

Impact: This made Docker self-sufficient, more secure, and highly consistent across all Linux distributions.

3️⃣ Standardization: runc and the OCI Standard (2015)
As the container ecosystem exploded, the industry realized that standardizing how containers were built and run was critical to avoid a "format war" (like Docker vs. CoreOS rkt).

The OCI: Docker, alongside other tech giants, formed the Open Container Initiative (OCI) to establish open industry standards around container formats and runtimes.

The Birth of runc: Docker took the core container-execution code out of libcontainer, refactored it, and donated it to the OCI. This standalone CLI tool was named runc.

Impact: Today, runc is the universal OCI-compliant runtime used to actually spawn and run containers at the lowest level.

4️⃣ Componentization: The Rise of containerd
As Docker transitioned to a modular architecture, it needed a dedicated daemon to manage the complete container lifecycle (image transfer, storage, and container execution via runc).

Enter containerd: Docker spun out its core container management logic into a standalone daemon called containerd.

Impact: Docker eventually donated containerd to the Cloud Native Computing Foundation (CNCF). It became so robust that Kubernetes later adopted it as its default container runtime (graduating away from Docker entirely).

5️⃣ Today: Docker Sandbox, VMMs, and Beyond
Modern Docker is no longer just a Linux daemon; it is a cross-platform powerhouse that focuses heavily on developer experience, security, and integration.

VMM (Virtual Machine Monitors): To run on macOS and Windows seamlessly, Docker Desktop utilizes VMMs (like Hyper-V/WSL2 on Windows, and the Apple Virtualization Framework on Mac). It provisions a lightweight, invisible Linux utility VM to host the Docker daemon and containerd.

Docker Sandbox & Security: As security demands have grown, the ecosystem now supports advanced sandboxing technologies. Tools like gVisor or Kata Containers can be swapped in for runc to provide strict, VM-level isolation for containers while keeping them lightweight.

Wasm Integration: Today, Docker is expanding to run WebAssembly (Wasm) modules alongside traditional Linux containers, pointing towards an even more lightweight and secure future!

[!NOTE]

Summary: Docker evolved from a monolithic LXC wrapper into a modular, standardized stack (Docker Engine -> containerd -> runc -> OS Kernel), prioritizing security, cross-platform compatibility, and open industry standards.
