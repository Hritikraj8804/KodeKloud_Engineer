# Instructions

On our `Storage` server in `Stratos Datacenter` we are having some issues where `nfsuser`user is holding hundred of processes, which is degrading the 
performance of the server. Therefore, we have a requirement to limit its maximum processes. Please set its maximum process limits as below:

a. soft limit = `1027`

b. hard_limit = `2025`

# Solution

ssh into the Storage server: `ssh natasha@172.16.238.15`

Edit the system limits configuration file: `sudo vi /etc/security/limits.conf`
