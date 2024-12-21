# Instructions

The Nautilus application development team has been working on a project repository `/opt/news.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`.  They recently shared the following requirements with DevOps team:

Create a new branch `xfusion` in `/usr/src/kodekloudrepos/news` repo from `master` and copy the `/tmp/index.html` file (present on `storage server` itself) into the repo. Further, `add/commit` this file in the new branch and merge back that branch into `master` branch. Finally, push the changes to the origin for both of the branches.

# Solution

`sudo su -`

`cd /usr/src/kodekloudrepos/news`

`git status`

`git checkout -b xfusion`

`git status`

`git branch`

`cp /tmp/index.html /usr/src/kodekloudrepos/news/`

`git add index.html`

`git commit -m "add index.html”`

`git checkout master`
