## Question
Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 2 in the Stratos Datacenter:

Install the required SELinux packages.
Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

## Answer

```bash
sudo yum install -y policycoreutils-python-utils
```

```bash
sudo vi /etc/selinux/config
```

```
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

```bash
grep "^SELINUX=" /etc/selinux/config
```
