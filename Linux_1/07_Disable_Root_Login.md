# Instructions

After doing some security audits of servers, `xFusionCorp Industries` security team has implemented some new security policies. One of them is to disable direct root login through SSH.

Disable direct SSH root login on all app servers in `Stratos Datacenter`.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

ssh into the App Server 2: `ssh steve@172.16.238.11`

ssh into the App Server 3: `ssh banner@172.16.238.12`

Open the SSH server configuration file (usually located at `/etc/ssh/sshd_config`) using a text editor such as `vi:` `sudo vi /etc/ssh/sshd_config`

Modify the value of `PermitRootLogin` to `no` or `without-password`. This disables direct root login.

- Setting it to `no` will completely disable root login.
- Setting it to `without-password` will allow root login only with public key authentication.
