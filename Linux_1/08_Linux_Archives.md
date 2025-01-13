# Instructions

On `Nautilus` storage server in `Stratos DC`, there is a storage location named `/data`, which is used by different developers to keep their data (non confidential data). One of the developers named `yousuf` has raised a ticket and asked for a copy of their data present in `/data/yousuf` directory on storage server. `/home` is a FTP location on storage server itself where developers can download their data. Below are the instructions shared by the system admin team to accomplish this task.

a. Make a `yousuf.tar.gz` compressed archive of `/data/yousuf` directory and move the archive to `/home` directory on Storage Server.

# Solution

ssh into the Nautilus Storage Server: `ssh natasha@172.16.238.15`

Check the files: `cd /data/yousuf`
