# 🐧 Docker Installation on Linux (Ubuntu / Debian)

![Linux](https://img.shields.io/badge/OS-Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-Engine-2496ED?style=for-the-badge&logo=docker&logoColor=white)

This guide walks you through the official and most secure way to install Docker Engine on a Linux system (specifically Ubuntu and Debian distributions) using the `apt` repository.

---

## 📋 Prerequisites

Before you begin, ensure you have:
- A 64-bit version of Ubuntu or Debian.
- A user account with `sudo` privileges.
- An active internet connection.

> [!WARNING]  
> **Uninstall Old Versions**  
> If you have older versions of Docker installed (called `docker`, `docker.io`, or `docker-engine`), uninstall them first:
> ```bash
> sudo apt-get remove docker docker-engine docker.io containerd runc
> ```

---
## 📺 Watch the Tutorial

Click the image below to watch the full Docker tutorial on YouTube!

[![Docker with Stan Tutorial](https://img.youtube.com/vi/MILDr6lqhcI/maxresdefault.jpg)](https://youtu.be/MILDr6lqhcI)

---

## 🚀 Step-by-Step Installation

### Step 1: Update your package database
Start by updating your local package index to ensure you get the latest versions.
```bash
sudo apt-get update

```

### Step 2: Install required dependencies

Install packages to allow `apt` to use a repository over HTTPS.

```bash
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

```

### Step 3: Add Docker’s official GPG key

Create the keyring directory and add Docker's official cryptographic key to verify the downloads.

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

```

### Step 4: Set up the Docker repository

Add the Docker repository to your system's software sources.

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

### Step 5: Install Docker Engine

Update the package index again (to read the newly added repository) and install Docker.

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

```

### Step 6: Verify the Installation

Run the built-in `hello-world` image to verify that Docker is installed correctly and can pull images.

```bash
sudo docker run hello-world

```

*If you see a "Hello from Docker!" message, your installation is completely successful!* 🎉

---

## ⚙️ Post-Installation Setup (Highly Recommended)

By default, the `docker` command can only be run by the **root** user or by a user with **sudo** privileges. To avoid typing `sudo` before every Docker command, you need to add your user to the `docker` group.

> [!NOTE]
> **Manage Docker as a non-root user**

**1. Create the docker group (if it doesn't already exist):**

```bash
sudo groupadd docker

```

**2. Add your current user to the docker group:**

```bash
sudo usermod -aG docker $USER

```

**3. Apply the new group membership:**
Log out and log back in, or run the following command to apply the changes immediately:

```bash
newgrp docker

```

**4. Test it out without `sudo`:**

```bash
docker run hello-world

```

---

## 🛠️ How to Enable Docker to Start on Boot

If you want Docker to automatically start whenever you reboot your Linux server, enable the Docker service using `systemctl`:

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service

```

*(To disable this behavior at any time, just replace `enable` with `disable`).*

```

```
