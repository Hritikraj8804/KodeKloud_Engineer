## Question
Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 2 in the Stratos Datacenter:

Install the required SELinux packages.
Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

## Answer

### 1\. Install SELinux Packages

First, connect to `App server 2` and use the `yum` package manager to install the necessary SELinux utilities. This command will install packages that provide tools for managing SELinux policies.

```bash
sudo yum install -y policycoreutils-python-utils
```

-----

### 2\. Permanently Disable SELinux

To permanently disable SELinux, you need to edit its main configuration file.

1.  Open the configuration file located at `/etc/selinux/config` using a text editor such as `vi` or `nano`.

    ```bash
    sudo vi /etc/selinux/config
    ```

2.  Find the line that starts with `SELINUX=` and change its value to `disabled`.

    ```ini
    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #     enforcing - SELinux security policy is enforced.
    #     permissive - SELinux prints warnings instead of enforcing.
    #     disabled - No SELinux policy is loaded.
    SELINUX=disabled
    # SELINUXTYPE= can take one of these three values:
    #     targeted - Targeted processes are protected,
    #     minimum - Modification of targeted policy. Only selected processes are protected.
    #     mls - Multi Level Security protection.
    SELINUXTYPE=targeted
    ```

3.  Save your changes and exit the text editor.

-----

### 3\. Verify the Configuration

You can confirm that the configuration file was updated correctly by running the following command.

```bash
grep "^SELINUX=" /etc/selinux/config
```

The expected output should be:

```
SELINUX=disabled
```

No reboot is necessary at this time. The server will start with SELinux disabled after the scheduled maintenance reboot.
