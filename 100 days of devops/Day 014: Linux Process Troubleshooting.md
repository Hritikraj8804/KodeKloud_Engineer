# Task

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.


Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 8083 on all app servers.

# Solution

###  ⚙️ Steps for Each App Server

Here is the repeatable process to apply to each server. We'll use `stapp01` as the first example.

1.  **Connect to the App Server**

    ```bash
    ssh tony@stapp01
    ```

2.  **Install Apache (httpd)**
    This ensures the package is present. If it's already installed, it will just confirm the latest version.

    ```bash
    sudo yum install httpd -y
    ```

3.  **Configure Apache Port**
    Edit the main configuration file to change the listening port to `8083`.

    ```bash
    sudo vi /etc/httpd/conf/httpd.conf
    ```

    Find the line `Listen 80` (usually around line 42) and change it to `Listen 8083`.

    ```conf
    # Before
    Listen 80

    # After
    Listen 8083
    ```

    Save and close the file (`:wq`).

4.  **Configure the Firewall**
    Add a rule to the firewall to allow incoming connections on the new port.

    ```bash
    # Add a permanent rule for port 8083
    sudo firewall-cmd --permanent --add-port=8083/tcp

    # Reload the firewall to apply the change
    sudo firewall-cmd --reload
    ```

5. 🛠️ The Fix: Find and Stop the Conflicting Process

To solve this, you need to find the other program and stop it so Apache can use the port.

1.  **Find the Conflicting Process**
    Use the `netstat` command to see which program is listening on port `8083`.

    ```bash
    sudo netstat -tlpn | grep 8083
    ```

    The output will show you the Process ID (PID) and the name of the application currently using that port.

2.  **Stop the Process**
    Once you have the PID from the command above, use the `kill` command to stop it. For example, if the PID is `1234`:

    ```bash
    sudo kill 1234
    ```


###  ✅ Verification

You can then run `sudo systemctl status httpd` to confirm it is **`active (running)`**.

6.  **Start and Enable Apache**
    This will start the service now and ensure it also starts automatically after a reboot.

    ```bash
    sudo systemctl start httpd
    sudo systemctl enable httpd
    ```

-----

### ✅ Final Action

Repeat the exact same steps for the remaining app servers:

  * **App Server 2:** `ssh steve@stapp02`
  * **App Server 3:** `ssh banner@stapp03`

After completing these steps on all three servers, the Apache service will be up, running, and listening on port `8083` across your infrastructure, resolving the issue.
