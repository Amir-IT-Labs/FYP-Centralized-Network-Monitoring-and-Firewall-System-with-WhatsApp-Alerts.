# 🐳 Docker Architecture and Deployment on Ubuntu Server

## Relationship Between Ubuntu Server and Docker

In this project, **Docker** is installed directly on **Ubuntu Server** and uses the Ubuntu Linux kernel to run containerized applications. Docker is **not** a separate operating system; instead, it serves as the container runtime responsible for deploying and managing application services.

The deployment hierarchy is shown below:

```text
Host Machine
      │
      ▼
VMware Workstation
      │
      ▼
Ubuntu Server
      │
      ▼
Docker Engine
      │
      ▼
Application Containers
      ├── Uptime Kuma
      └── WAHA
```

This layered architecture provides clear separation between the host operating system and application services while maintaining high performance and resource efficiency.

---

# What is Docker?

Docker is a **containerization platform** that enables applications to run inside lightweight, isolated environments called **containers**.

Unlike traditional virtual machines, Docker containers:

* Share the Ubuntu Server kernel
* Do not require a separate operating system
* Start within seconds
* Consume significantly fewer system resources

Docker depends on Ubuntu Server and cannot function independently without the host operating system.

---

# Purpose of Docker in This Project

Docker is used to provide isolated environments for monitoring and alerting services.

Its primary purposes include:

* Running monitoring services without installing them directly on Ubuntu Server
* Preventing dependency conflicts
* Simplifying deployment and updates
* Isolating each service into its own container

Ubuntu Server provides the operating system, while Docker provides application-level isolation.

---

# Why Docker Instead of Native Installation?

Installing services directly on Ubuntu Server may introduce:

* Package conflicts
* Difficult upgrades
* Risk of affecting the base operating system

Docker addresses these issues by:

* Packaging each application inside its own container
* Keeping the Ubuntu Server clean and lightweight
* Allowing services to be updated or restarted independently

This makes Docker ideal for production-style deployments.

---

# Docker vs Virtual Machines

| Feature               | Docker Containers | Virtual Machines |
| --------------------- | ----------------- | ---------------- |
| Uses Ubuntu Kernel    | ✅ Yes             | ❌ No             |
| Startup Time          | Seconds           | Minutes          |
| Resource Usage        | Low               | High             |
| Full Operating System | No                | Yes              |

In this project:

* Ubuntu Server runs inside a VMware virtual machine.
* Docker runs inside Ubuntu Server.
* Monitoring services run inside Docker containers.

---

# Installing Docker on Ubuntu Server

Docker was installed remotely using an SSH session from Windows PowerShell.

Connect to Ubuntu Server:

```bash
ssh amir@192.168.100.10
```

After login, all installation commands were executed remotely.

---

# Update Ubuntu Packages

Before installing Docker, update the package index:

```bash
sudo apt update
```

This ensures Ubuntu retrieves the latest package information.

---

# Install Required Dependencies

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

These packages enable Ubuntu Server to securely communicate with external repositories.

---

# Add Docker GPG Key

```bash
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

The Docker GPG key verifies the authenticity of Docker packages before installation.

---

# Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Then update the package list again:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

This installs:

* Docker Engine
* Docker CLI
* Container Runtime
* Docker Compose Plugin

---

# Verify Docker Installation

Check the installed Docker version:

```bash
docker --version
```

Verify the Docker service:

```bash
sudo systemctl status docker
```

A successful output confirms that Docker is installed and running correctly.

---

# Server Access Policy

Ubuntu Server is managed **entirely through SSH**.

Connect using:

```bash
ssh amir@192.168.100.10
```

The administrative user **amir** performs all server management tasks.

This includes:

* Docker management
* Monitoring services
* System maintenance
* Container deployment

No graphical interface or local console is used after installation.

---

# Benefits of Remote Administration

Managing the server over SSH provides several advantages:

* Centralized administration
* Reduced physical attack surface
* No GUI overhead
* Consistent server management
* Enterprise-style administration practices

---

# Docker Services Used

The following Docker containers are deployed:

| Container       | Purpose            |
| --------------- | ------------------ |
| **Uptime Kuma** | Network Monitoring |
| **WAHA**        | WhatsApp HTTP API  |

Each container:

* Runs independently
* Uses only required ports
* Can be restarted without affecting Ubuntu Server

---

# Port Exposure

Docker provides controlled port mapping between the host system and application containers.

Benefits include:

* Only required ports are exposed
* Internal container ports remain isolated
* Reduced attack surface
* Simplified network management

---

# Security Benefits

Using Docker inside Ubuntu Server provides several security advantages:

* Reduced host operating system attack surface
* Isolation of third-party applications
* Controlled service access
* Easier auditing and logging
* Independent application lifecycle management

---

# Summary

Docker serves as the containerization layer within Ubuntu Server, enabling secure and isolated deployment of monitoring and alerting services.

The implemented architecture provides:

* Lightweight application deployment
* Service isolation
* Simplified maintenance
* Improved scalability
* Enhanced security
* Efficient resource utilization

Together, Ubuntu Server and Docker create a reliable and production-ready platform for the monitoring and alerting system used in this project.
