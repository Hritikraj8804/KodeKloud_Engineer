# Instructions

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/ecommerce`. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:

1. On `Storage server` in Stratos DC create a new branch `xfusioncorp_ecommerce` from `master` branch in `/usr/src/kodekloudrepos/ecommerce` git repo. 
2. Please do not try to make any changes in the code.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`cd /usr/src/kodekloudrepos/ecommerce`

`git checkout master`

`git checkout -b xfusioncorp_ecommerce`

`git status`

`git branch -a`

<img width="563" height="331" alt="image" src="https://github.com/user-attachments/assets/fb9d213e-b848-4405-a02b-752ac1916fb9" />
