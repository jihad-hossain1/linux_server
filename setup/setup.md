Setting up **Fedora 42 Server Edition** involves installing the OS and configuring basic settings. Here’s a step-by-step guide for setting up Fedora Server Edition on a **bare metal machine**, **VM**, or **cloud/VPS**.

---

## 🖥️ Fedora 42 Server Setup Guide

---

### ✅ 1. **Download Fedora Server ISO**

* Go to: [https://getfedora.org/en/server/](https://getfedora.org/en/server/)
* Download the **Fedora 42 Server Edition ISO** (`.iso`)

---

### ✅ 2. **Create Bootable USB (if installing on hardware)**

Use tools like:

* **Linux**: `dd` or `balenaEtcher`
* **Windows**: [Rufus](https://rufus.ie)

Example (Linux terminal):

```bash
sudo dd if=fedora-server.iso of=/dev/sdX bs=4M status=progress && sync
```

> Replace `/dev/sdX` with your actual USB device.

---

### ✅ 3. **Boot the Installer**

* Insert USB and boot the system.
* Select **Install Fedora Server**.

---

### ✅ 4. **Fedora Server Installation Steps**

#### a. **Language Selection**

Choose your preferred language and continue.

#### b. **Installation Destination**

Select disk and choose:

* **Automatic partitioning** (recommended)
* Or manual (for experienced users)

#### c. **Network & Hostname**

Set:

* System hostname (e.g., `server.localdomain`)
* Enable networking

#### d. **Root Password**

Set a strong **root** password.

#### e. **User Account (Recommended)**

Create a new **administrator (sudo)** user (e.g., `jihad`)

---

### ✅ 5. **Begin Installation**

Click **Begin Installation** and wait for completion.

Once done, **reboot**.

---

## 🚀 Post-Install Setup

### 1. **Login**

Use either the root or user credentials created during install.

### 2. **Update System**

```bash
sudo dnf update -y
```

### 3. **Enable Cockpit Web UI (optional)**

```bash
sudo systemctl enable --now cockpit.socket
```

* Access in browser: `https://<server-ip>:9090`

### 4. **Enable SSH (if needed)**

```bash
sudo systemctl enable --now sshd
```

### 5. **Install Common Tools**

```bash
sudo dnf install -y vim git htop net-tools
```

---

## 🔐 Security Hardening

* **Add a firewall rule:**

  ```bash
  sudo firewall-cmd --add-service=ssh --permanent
  sudo firewall-cmd --reload
  ```
* **Create new user (if not done):**

  ```bash
  sudo useradd -m jihad
  sudo passwd jihad
  sudo usermod -aG wheel jihad  # give sudo access
  ```

---

## ✅ Check Your Setup

Run:

```bash
hostnamectl
ip a
```

To verify hostname and IP settings.

---

