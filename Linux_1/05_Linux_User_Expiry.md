# Instructions

A developer named kirsty has been assigned `Nautilus` project temporarily as a backup resource. As a temporary resource for this project, we need a temporary user for kirsty. It’s a good idea to create a user with an expiration date so that the user won't be able to access servers beyond that point.

Therefore, create a user named `kirsty` on the `App Server 1` in `Stratos Datacenter`. Set `expiry date` to `2021-02-17`. Make sure the user is created as per standard and is in lowercase.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Run the following command to create the user "kirsty" with a lowercase username: `sudo useradd --expiredate 2021-02-17 --create-home --shell /bin/bash kirsty`

To check if the user "kirsty" was created, you can use the `grep` command to search for the username in the `/etc/passwd` file: `grep "^kirsty:" /etc/passwd`

If the user was created successfully, you should see output similar to the following: `kirsty:x:1001:1001::/home/kirsty:/bin/bash`
