# Instructions

The `Nautilus` system admins team recently deployed a web UI application for their backup utility running on the `Nautilus backup server` in `Stratos Datacenter`. The application is running on port `8089`. They have `firewalld` installed on that server. The requirements that have come up include the following:

Open all incoming connection  on `8089/tcp` port.  Zone should be `public`.

# Solution

ssh into the Backup server: `ssh clint@172.16.238.16`
