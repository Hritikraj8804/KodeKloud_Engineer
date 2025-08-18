# Task

Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:





a. Install nginx on LBR (load balancer) server.



b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.



c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.



d. Once done, you can access the website using StaticApp button on the top bar.

# Solution

Of course. Here is a step-by-step guide to configure the Nginx load balancer to distribute traffic across your app servers.

-----

### \#\# 1. Prepare App Servers

First, ensure the Apache service (`httpd`) is running on all three app servers and identify the port it's using. You'll need to SSH into each server to perform these checks.

1.  **SSH into each App Server** (`stapp01`, `stapp02`, `stapp03`). For example:
    ```bash
    ssh tony@stapp01
    ```
2.  **Check Apache Status**:
    ```bash
    sudo systemctl status httpd
    ```
    If the service is inactive, start it:
    ```bash
    sudo systemctl start httpd
    ```
3.  **Identify the Apache Port**:
    Use the `ss` or `netstat` command to find the port `httpd` is listening on. This is a crucial step.
    ```bash
    sudo ss -tlpn | grep httpd
    ```
    Note the port number (e.g., `8080`, `8083`, etc.), as you will need it for the Nginx configuration. For this guide, we'll assume the port is **8080**.

-----

### \#\# 2. Configure the Nginx Load Balancer

Now, connect to the LBR server, install Nginx, and configure the load balancing.

1.  **SSH into the LBR Server (`stlb01`)**:

    ```bash
    ssh loki@stlb01
    ```

2.  **Install Nginx**:

    ```bash
    sudo yum install nginx -y
    ```

3.  **Backup and Edit the Configuration File**:
    It's always a good practice to back up the original configuration before making changes.

    ```bash
    sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
    sudo vi /etc/nginx/nginx.conf
    ```

4.  **Add the `upstream` Block**:
    Inside the `http { ... }` block, and **before** the `server { ... }` block, add an `upstream` block. This block defines the pool of backend app servers.

    ```nginx
    http {
        # ... other http configurations ...

        upstream backend_servers {
            server 172.16.238.10:8080;  # App Server 1
            server 172.16.238.11:8080;  # App Server 2
            server 172.16.238.12:8080;  # App Server 3
        }

        server {
            # ... server configuration ...
        }
        # ... other http configurations ...
    }
    ```

    **Note:** Replace `8080` with the actual port you identified in the first phase.

5.  **Modify the `location` Block**:
    Find the existing `server { ... }` block. Inside it, locate the `location / { ... }` section and modify it to use the `proxy_pass` directive, pointing to your new `upstream` block.

    ```nginx
    server {
        listen       80 default_server;
        # ... other server configurations ...

        location / {
            # Replace existing content (like root and index) with this line:
            proxy_pass http://backend_servers;
        }

        # ... other server configurations ...
    }
    ```

    Save and close the file (`:wq`).

-----

### \#\# 3. Start and Verify

Finally, test your configuration and start the Nginx service.

1.  **Test Nginx Configuration**:
    This command will check for any syntax errors in your file.

    ```bash
    sudo nginx -t
    ```

    You should see the output `syntax is ok` and `test is successful`.

2.  **Start and Enable Nginx**:

    ```bash
    sudo systemctl start nginx
    sudo systemctl enable nginx
    ```

Your load balancer is now configured. You can use the **StaticApp** button on the top bar to verify that traffic is being correctly routed to your app servers.
