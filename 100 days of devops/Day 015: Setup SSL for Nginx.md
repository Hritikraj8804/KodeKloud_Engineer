# Task

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:





1. Install and configure nginx on App Server 2.



2. On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.



3. Create an index.html file with content Welcome! under Nginx document root.



4. For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.


# Solution

### \#\# 📦 1. Install Nginx and Create Web Content

First, connect to the app server, install the Nginx package, and create the simple homepage.

1.  **SSH into App Server 2 (`stapp02`)**:
    ```bash
    ssh steve@172.16.238.11
    ```
2.  **Install Nginx**:
    ```bash
    sudo yum install nginx -y
    ```
3.  **Create the `index.html` file**:
    This command places a file with the content "Welcome\!" in the default Nginx webroot.
    ```bash
    echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
    ```

-----

### \#\# 🔐 2. Secure and Deploy SSL Certificate

Next, move the SSL certificate and private key to their standard, secure locations and set the correct permissions.

1.  **Create Directories for SSL**:
    ```bash
    sudo mkdir -p /etc/ssl/certs /etc/ssl/private
    ```
2.  **Move the Certificate and Key**:
    ```bash
    sudo mv /tmp/nautilus.crt /etc/ssl/certs/nautilus.crt
    sudo mv /tmp/nautilus.key /etc/ssl/private/nautilus.key
    ```
3.  **Set Secure Permissions**:
    The private key should only be readable by the root user for security.
    ```bash
    sudo chmod 600 /etc/ssl/private/nautilus.key
    ```

-----

### \#\# ⚙️ 3. Configure Nginx for SSL

Now, create a new Nginx configuration file to enable HTTPS on port 443.

1.  **Create a New Nginx Configuration File**:

    ```bash
    sudo vi /etc/nginx/conf.d/ssl.conf
    ```

2.  **Add the following Server Block**:
    Paste this configuration into the file. It tells Nginx to listen for HTTPS traffic and where to find the SSL certificate, key, and website files.

    ```nginx
    server {
        listen       443 ssl;
        server_name  stapp02.stratos.xfusioncorp.com;

        ssl_certificate      /etc/ssl/certs/nautilus.crt;
        ssl_certificate_key  /etc/ssl/private/nautilus.key;

        location / {
            root   /usr/share/nginx/html;
            index  index.html;
        }
    }
    ```

    Save and exit the editor (`:wq`).

-----

### \#\# 🔥 4. Update Firewall and Start Nginx

Finally, open the HTTPS port in the firewall and start the Nginx service.

1.  **Open Port 443 in the Firewall**:

    ```bash
    sudo firewall-cmd --permanent --add-service=https
    sudo firewall-cmd --reload
    ```

2.  **Test Nginx Configuration**:
    Always test your configuration before starting the service.

    ```bash
    sudo nginx -t
    ```

    You should see `syntax is ok` and `test is successful`.

3.  **Start and Enable Nginx**:

    ```bash
    sudo systemctl start nginx
    sudo systemctl enable nginx
    ```

-----

### \#\# ✅ 5. Final Verification

Return to the **jump host** to perform the final test.

1.  **Exit from App Server 2**:
    ```bash
    exit
    ```
2.  **Test with `curl`**:
    Run the `curl` command from the jump host. The `-k` flag is important because it tells curl to ignore errors from the self-signed certificate.
    ```bash
    curl -Ik https://172.16.238.11/
    ```
    You should receive an `HTTP/1.1 200 OK` response, confirming that your setup is working correctly.
