# Task

The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:





a. Install nginx on app server 3 , configure it to use port 8093 and its document root should be /var/www/html.



b. Install php-fpm version 8.1 on app server 3, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).



c. Configure php-fpm and nginx to work together.



d. Once configured correctly, you can test the website using curl http://stapp03:8093/index.php command from jump host.

NOTE: We have copied two files, index.php and info.php, under /var/www/html as part of the PHP-based application setup. Please do not modify these files.


# Solutiion

### **1. Connect to the Server**

First, connect to the designated application server from the jump host.

```bash
ssh banner@stapp03
```

### **2. Install Nginx and PHP-FPM**

Switch to the root user for installation and configuration.

```bash
sudo -i
```

Install the Nginx web server.

```bash
yum install nginx -y
```

To install the specific PHP version, you'll need to enable the Remi repository.

```bash
yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
yum module reset php -y
yum module enable php:remi-8.1 -y
yum install php-fpm -y
```

-----

### **3. Configure PHP-FPM**

Modify the default PHP-FPM pool configuration file to meet the requirements.

```bash
vi /etc/php-fpm.d/www.conf
```

Make the following changes inside the `www.conf` file:

  * Set the **user** and **group** to `nginx`.
    ```ini
    user = nginx
    group = nginx
    ```
  * Set the `listen` directive to use the specified Unix socket.
    ```ini
    listen = /var/run/php-fpm/default.sock
    ```
  * Set the socket **owner** and **group** to `nginx`.
    ```ini
    listen.owner = nginx
    listen.group = nginx
    ```

Save and close the file.

-----

### **4. Configure Nginx**

Now, configure Nginx to serve the PHP application on port `8093` and communicate with PHP-FPM. Edit the main Nginx configuration file.

```bash
vi /etc/nginx/nginx.conf
```

Modify the default `server` block within the `http` block as follows. You can comment out or delete the original `server` block and replace it with this one.

```nginx
server {
    listen       8093;
    server_name  stapp03;
    root         /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include        fastcgi_params;
        fastcgi_pass   unix:/var/run/php-fpm/default.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

Save and close the file. Check the Nginx configuration for syntax errors.

```bash
nginx -t
```

You should see a message indicating the syntax is okay and the test is successful.

-----

### **5. Start and Enable Services**

Start both the `nginx` and `php-fpm` services and enable them to launch on system boot.

```bash
systemctl start nginx
systemctl enable nginx

systemctl start php-fpm
systemctl enable php-fpm
```

### **6. Verification**

Log out of `stapp03` to return to the jump host. Test the setup using `curl`.

```bash
curl http://stapp03:8093/index.php
```

You should see the output from the PHP script, which confirms that Nginx is correctly processing `.php` files through PHP-FPM. You can also test the `info.php` page.

```bash
curl http://stapp03:8093/info.php
```
# Error you may face:
[root@stapp03 ~]# yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm

Last metadata expiration check: 0:01:34 ago on Fri Sep 12 15:46:57 2025.

remi-release-8.rpm                                        62 kB/s |  37 kB     00:00    

Error: 

 Problem: conflicting requests

  - nothing provides epel-release = 8 needed by remi-release-8.10-2.el8.remi.noarch from @commandline

  - nothing provides system-release(releasever) = 8 needed by remi-release-8.10-2.el8.remi.noarch from @commandline

(try to add '--skip-broken' to skip uninstallable packages or '--nobest' to use not only best candidate packages)

[root@stapp03 ~]# 

## try this
That's the key piece of information we needed\!

The output `Package epel-release-9-10.el9.noarch is already installed` confirms your server is an **Enterprise Linux 9** system (like CentOS Stream 9, RHEL 9, Rocky 9, etc.).

The problem is that you've been trying to install the Remi repository for version **8**. You need to use the link for version **9** to match your operating system.

-----

### \#\# The Solution

Use the correct URL for Enterprise Linux 9 to install the Remi repository.

```bash
yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

This command will install the version of the Remi repository that matches your system. After this, you can proceed with enabling the PHP module and installing `php-fpm`.
