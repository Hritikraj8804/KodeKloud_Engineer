# Instructions

For security reasons the `xFusionCorp Industries` security team has decided to use custom Apache users for each web application hosted, rather than its default user. This will be the Apache user, so it shouldn't use the default home directory. Create the user as per requirements given below:

a. Create a user named `jim` on the `App server 2` in Stratos Datacenter.

b. Set its UID to `1582` and home directory to `/var/www/jim`.

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

This command creates a new user with the username "jim", sets the UID to 1582, creates the home directory `/var/www/jim`, and the `-m` option ensures that the home directory is created: `sudo useradd -u 1582 -d /var/www/jim -m jim` 
