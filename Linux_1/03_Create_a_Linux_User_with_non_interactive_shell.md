# Instructions

The System admin team of `xFusionCorp Industries` has installed a backup agent tool on all app servers. As per the tool's requirements they need to create a user with a non-interactive shell.

Therefore, create a user named `mariyam` with a non-interactive shell on the `App Server 1`.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Run the following command to create the user "mariyam" with a non-interactive shell: `sudo useradd --shell /bin/false mariyam`
