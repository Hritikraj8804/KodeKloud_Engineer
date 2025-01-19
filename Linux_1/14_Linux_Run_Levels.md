# Instructions

New tools have been installed on the app servers in `Stratos Datacenter`.Some of these tools can only be managed from the graphical user interface. Therefore, there are some requirements for these app servers as below.

On all App servers in `Stratos Datacenter`, change the `default runlevel` so that they can boot in `GUI (graphical user interface)` by default. Please do not try to `reboot` these servers after completing this task.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

ssh into the App Server 2: `ssh steve@172.16.238.11`

ssh into the App Server 3: `ssh banner@172.16.238.12`

Switch to the root user: `sudo su -`
