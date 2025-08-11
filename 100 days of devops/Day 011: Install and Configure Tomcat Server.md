# Task

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:





a. Install tomcat server on App Server 1.



b. Configure it to run on port 8088.



c. There is a ROOT.war file on Jump host at location /tmp.



Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp01:8088

# Tomcat Application Deployment on App Server 1

This document outlines the process for installing and configuring an Apache Tomcat server on App Server 1 (`stapp01`) in the Stratos DC. The goal is to deploy a Java-based web application and make it accessible on a custom port.

-----

## 🎯 Objective

To set up a fully functional Tomcat environment on `stapp01` that serves a specific Java application.

### Requirements

  * Install the **Tomcat** server on **App Server 1**.
  * Configure the server to listen on port **8088**.
  * Deploy the `ROOT.war` application, which is located on the Jump Host.
  * Ensure the application is accessible at the base URL (e.g., `http://stapp01:8088`).

-----

## Solution Steps

The solution is broken down into four distinct phases: Installation, Configuration, Deployment, and Verification.

### 📦 Phase 1: Install Tomcat

1.  **Connect to App Server 1** from the Jump Host.
    ```bash
    ssh tony@172.16.238.10
    ```
2.  **Install the Tomcat package** using the `yum` package manager.
    ```bash
    sudo yum install tomcat -y
    ```

### ⚙️ Phase 2: Configure Port

1.  **Edit the `server.xml` file** to change the default port.
    ```bash
    sudo vi /usr/share/tomcat/conf/server.xml
    ```
2.  **Locate the HTTP `<Connector>`** element and change its `port` attribute from `8080` to `8088`.
    ```xml
    <Connector port="8080" protocol="HTTP/1.1" ... />

    <Connector port="8088" protocol="HTTP/1.1" ... />
    ```

### 🚀 Phase 3: Deploy Application

1.  **Copy the `ROOT.war` file** from the Jump Host to App Server 1. You will need to `exit` back to the Jump Host to run this `scp` command.
    ```bash
    scp /tmp/ROOT.war tony@172.16.238.10:/tmp/
    ```
2.  **Move the application** into Tomcat's deployment directory. Log back into App Server 1 to perform this step. Naming the file `ROOT.war` ensures it deploys to the base URL.
    ```bash
    # SSH back into stapp01
    ssh tony@172.16.238.10

    # Move the file to the webapps directory
    sudo mv /tmp/ROOT.war /usr/share/tomcat/webapps/
    ```

### ✅ Phase 4: Start and Verify

1.  **Start and enable the Tomcat service** on App Server 1.
    ```bash
    sudo systemctl start tomcat
    sudo systemctl enable tomcat
    ```
2.  **Verify the deployment** by making a request to the server. A successful command will return the application's HTML content.
    ```bash
    curl http://stapp01:8088
    ```
