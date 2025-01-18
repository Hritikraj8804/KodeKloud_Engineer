# Instructions

To stick with the security compliances, the Nautilus project team has decided to apply some restrictions on crontab access so that only allowed users can create/update the cron jobs. Limit crontab access to below specified users on `App Server 2`.

Allow crontab access to `rose` user and deny the same to `jerome` user.

# Solution

ssh into the App Server 2: `ssh steve@172.16.238.11`

`sudo su`

By executing this command with root privileges, the "rose" user will be allowed to use crontab: `echo rose >> /etc/cron.allow`

By executing this command, the "jerome" user will be denied crontab access: `echo jerome >> /etc/cron.deny`

check results with `cat`:

`sudo systemctl restart crond.service`

`sudo systemctl status crond.service`
