# Instructions

Last week the Nautilus DevOps team met with the application development team and decided to containerize several of their applications. The DevOps team wants to do some testing per the following:

1. Install `docker-ce` and `docker-compose` packages on `App Server 1`.
2. Start `docker` service.

# Solution

It seems you are running commands on a Linux distribution that uses the **`yum`** or **`dnf`** package manager (like **CentOS**, **RHEL**, or **Fedora**) rather than the **`apt-get`** package manager (used by **Ubuntu** or **Debian**). The original instructions were likely tailored for Debian/Ubuntu.

You'll need to use the appropriate package management commands for your system to install Docker CE and Docker Compose.

Here are the revised steps for a **RHEL/CentOS/Fedora** system:

## 1\. Install Docker Engine (docker-ce)

### A. Set up the Docker repository

1.  **Install yum-utils and set up the stable repository:**
    ```bash
    sudo yum install -y yum-utils
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```
    (Use `dnf` instead of `yum` for recent Fedora/RHEL versions.)

### B. Install Docker CE and the Compose plugin

Starting with Docker Engine 19.03, **Docker Compose is available as a plugin** (`docker-compose-plugin`) and is the recommended way to install it.

2.  **Install the necessary packages:**
    ```bash
    sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
    (Again, use `dnf` instead of `yum` for recent Fedora/RHEL versions.)

-----

## 2\. Initiate the Docker Service

3.  **Start the Docker service:**

    ```bash
    sudo systemctl start docker
    ```

4.  **Enable Docker to start on boot:**

    ```bash
    sudo systemctl enable docker
    ```

5.  **Check the service status (optional):**

    ```bash
    sudo systemctl status docker
    ```

-----

## 3\. Verify the Installation

You can confirm both are installed:

  * **Docker Engine:**
    ```bash
    sudo docker --version
    ```
  * **Docker Compose Plugin:**
    ```bash
    sudo docker compose version
    ```
    (Note the new command syntax: `docker compose` instead of `docker-compose`).
