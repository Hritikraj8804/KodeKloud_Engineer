# Instructions

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/apps`. They were doing some testing and created few test branches, now they want to clean those test branches. Below are the requirements that have been shared with the DevOps team:

On `Storage server` in Stratos DC delete a branch named `xfusioncorp_apps` from `/usr/src/kodekloudrepos/apps` git repo.

# Solution

ssh into the Storage server: `ssh natasha@ststor01`

`cd /usr/src/kodekloudrepos/apps`

`git config --global --add safe.directory /usr/src/kodekloudrepos/apps`

Check if this branch is there `git branch --list xfusioncorp_apps`

Check the branches `git branch`

Change to the master `sudo git checkout master`

Change the permissions:

`sudo chown -R natasha /usr/src/kodekloudrepos/apps`

`sudo chmod -R u+rwx /usr/src/kodekloudrepos/apps`
