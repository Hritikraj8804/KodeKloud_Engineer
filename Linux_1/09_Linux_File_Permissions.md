# Instructions

There are new requirements to automate a backup process that was performed manually by the `xFusionCorp Industries` system admins team earlier. To automate this task, the team has developed a new bash script `xfusioncorp.sh` They have already copied the script on all required servers, however 
they did not make it executable on one the app server i.e `App Server 1` in `Stratos Datacenter`.

Please give executable permissions to `/tmp/xfusioncorp.sh` script on `App Server 1`.  Also make sure every user can execute it.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Check files: `cd /tmp`

Change permissions: `sudo chmod +rx xfusioncorp.sh`

- `chmod`: The command used to change file permissions.
- `+rx`: The `+` sign grants the specified permissions, and `r` and `x` represent read and execute permissions, respectively. By using `+rx`, you are granting both read and execute permissions to the file.
- `file_name`: The name of the file you want to modify the permissions for.

Note: To make a file executable, it must also be readable.
