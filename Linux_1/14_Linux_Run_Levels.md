# Instructions

New tools have been installed on the app servers in `Stratos Datacenter`.Some of these tools can only be managed from the graphical user interface. Therefore, there are some requirements for these app servers as below.

On all App servers in `Stratos Datacenter`, change the `default runlevel` so that they can boot in `GUI (graphical user interface)` by default. Please do not try to `reboot` these servers after completing this task.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

ssh into the App Server 2: `ssh steve@172.16.238.11`

ssh into the App Server 3: `ssh banner@172.16.238.12`

Switch to the root user: `sudo su -`

The command will display the current default target, which can be `graphical.target` for GUI mode or `multi-user.target` for non-GUI mode: `systemctl get-default`

By changing the default target to `graphical.target`, the system will boot into the graphical user interface (GUI) by default when it starts up: `systemctl set-default graphical.target`

It will display the updated default target, which should now be `graphical.target` if the previous command was executed successfully: `systemctl get-default`

I got an error with App Server 3 :

`Failed to set default target: The name org.freedesktop.PolicyKit1 was not provided by any .service files`

