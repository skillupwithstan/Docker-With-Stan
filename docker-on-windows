---

### `Installation_Windows.md`

```markdown
# 🪟 Docker Installation on Windows

![Windows](https://img.shields.io/badge/OS-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Docker Desktop](https://img.shields.io/badge/Docker-Desktop-2496ED?style=for-the-badge&logo=docker&logoColor=white)

The easiest and officially supported way to run Docker on Windows is by installing **Docker Desktop**. It utilizes the **WSL 2 (Windows Subsystem for Linux)** backend to run Linux containers natively on Windows.

---

## 📋 System Prerequisites

Before installing, ensure your system meets the following requirements:
* **OS:** Windows 11 64-bit or Windows 10 64-bit (Home, Pro, Enterprise, or Education).
* **RAM:** At least 4GB of system memory.
* **Hardware Virtualization:** Must be enabled in your BIOS/UEFI settings.

> [!WARNING]  
> **Enable Virtualization**  
> To check if virtualization is enabled, open **Task Manager** (`Ctrl + Shift + Esc`), navigate to the **Performance** tab, and look for `Virtualization: Enabled` in the CPU section. If it says Disabled, you will need to enable it in your BIOS.

---

## 🚀 Step-by-Step Installation

### Step 1: Download Docker Desktop
1. Navigate to the official Docker website: [Download Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/).
2. Click the **"Docker Desktop for Windows"** button to download the installer (`.exe` file).

### Step 2: Run the Installer
1. Double-click the downloaded `Docker Desktop Installer.exe` to run it.
2. When prompted on the Configuration screen, ensure the **"Use WSL 2 instead of Hyper-V"** option is **checked** (this is the default and highly recommended setting).
3. Click **Ok** and wait for the installation process to unpack files and complete.

### Step 3: Restart Your Computer
Once the installation is successful, you will be prompted to restart your PC. 
* Click **Close and restart**. *(Do not skip this step!)*

### Step 4: Accept Terms and Start Docker
1. After your computer restarts, search for **Docker Desktop** in the Windows Start menu and open it.
2. You will be prompted to accept the Docker Subscription Service Agreement. Click **Accept**.
3. Docker Desktop will start initializing. You will see a whale icon in your system tray (bottom-right corner of your screen). When the icon stops animating and turns solid, the Docker Engine is running!

> [!NOTE]  
> **WSL 2 Update Prompt?**  
> If Docker prompts you to update the WSL 2 kernel upon first launch, simply follow the link provided in the prompt, download the Microsoft MSI update packet, run it, and restart Docker Desktop.

---

## 🧪 Verify the Installation

To ensure Docker is installed and communicating with your system correctly, open your preferred terminal (**Command Prompt**, **PowerShell**, or **Windows Terminal**) and run:

```powershell
docker --version

```

*This should output your installed Docker version.*

Next, pull and run the test container:

```powershell
docker run hello-world

```

*If everything is set up perfectly, Docker will download a small test image and print a "Hello from Docker!" success message in your terminal.* 🎉

---

## ⚙️ Important Settings to Check

Once Docker Desktop is running, click the **Gear icon (⚙️)** in the top right corner to access Settings:

1. **Start Docker Desktop when you log in:** Under the `General` tab, check this box if you want Docker ready to go immediately upon boot.
2. **Resource Allocation:** If you are using WSL 2, Windows manages CPU/Memory dynamically. If you are using the older Hyper-V backend, you can manually limit how much CPU and RAM Docker is allowed to use under the `Resources` tab.

```

```
