# Task
xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:





a. Install httpd package and dependencies on app server 2.



b. Apache should serve on port 6300.



c. There are two website's backups /home/thor/media and /home/thor/games on jump_host. Set them up on Apache in a way that media should work on the link http://localhost:6300/media/ and games should work on link http://localhost:6300/games/ on the mentioned app server.



d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:6300/media/ and curl http://localhost:6300/games/

# Solution

### Step 1: On the `jump_host`

This single command copies the website directories from the `jump_host` to `app server 2`.


```bash
scp -r /home/thor/media /home/thor/games steve@stapp02:/tmp
```

-----

### Step 2: On `app server 2`

After copying the files, SSH into `app server 2` and run the following commands in order.

1.  **Install Apache (httpd):**

    ```bash
    sudo yum install -y httpd
    ```

2.  **Configure Apache to use port 6300:**

    ```bash
    sudo sed -i 's/Listen 80/Listen 6300/g' /etc/httpd/conf/httpd.conf
    ```

3.  **Move the website files to the Apache web directory:**

    ```bash
    sudo mv /tmp/media /var/www/html/
    sudo mv /tmp/games /var/www/html/
    ```

4.  **Start and enable the Apache service:**

    ```bash
    sudo systemctl start httpd
    sudo systemctl enable httpd
    ```

-----

### Step 3: Verify the Setup (on `app server 2`)

Finally, run these `curl` commands on `app server 2` to confirm that both websites are being served correctly.

1.  **Check the `media` website:**

    ```bash
    curl http://localhost:6300/media/
    ```

2.  **Check the `games` website:**

    ```bash
    curl http://localhost:6300/games/
    ```

After running these commands, your setup will be complete and verified.
