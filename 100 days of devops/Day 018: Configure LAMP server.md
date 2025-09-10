# Task

xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:





a. Install httpd, php and its dependencies on all app hosts.



b. Apache should serve on port 6100 within the apps.



c. Install/Configure MariaDB server on DB Server.



d. Create a database named kodekloud_db5 and create a database user named kodekloud_pop identified as password BruCStnMT5. Further make sure this newly created user is able to perform all operation on the database you created.



e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud_pop

# Solution


You need to run the installation and configuration commands from the first section of the original script on each app servers.

Here are the commands you need to run on **each app host**:

1.  **Install httpd, php, and dependencies:**

    ```bash
    sudo yum install -y httpd php php-mysqlnd
    ```

2.  **Configure Apache to listen on port 6100:**

    ```bash
    sudo sed -i 's/Listen 80/Listen 6100/g' /etc/httpd/conf/httpd.conf
    ```

3.  **Start and enable the Apache service:**

    ```bash
    sudo systemctl start httpd
    sudo systemctl enable httpd
    ```

### 1\. Install MariaDB Server

This command installs the MariaDB database package.

```bash
sudo yum install -y mariadb-server
```

### 2\. Start and Enable the MariaDB Service

This ensures the database server starts now and also automatically after a reboot.

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

### 3\. Create the Database and User

This single command block will log into MariaDB as the root user and execute the required SQL to create your database, create the user, and assign the correct permissions. Just copy and paste the entire block into your terminal and press Enter.

```bash
sudo mysql -u root <<MYSQL_SCRIPT
CREATE DATABASE kodekloud_db5;
CREATE USER 'kodekloud_pop'@'%' IDENTIFIED BY 'BruCStnMT5';
GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_pop'@'%';
FLUSH PRIVILEGES;
MYSQL_SCRIPT
```

After running these commands on your database server, the MariaDB part of the task will be complete.
