# Instructions

DevOps team created a new Git repository last week; however, as of now 
no team is using it. The Nautilus application development team recently 
asked for a copy of that repo on `Storage server` in Stratos DC. Please clone the repo as per details shared below:

1. The repo that needs to be cloned is `/opt/beta.git`
2. Clone this git repository under `/usr/src/kodekloudrepos` directory. Please do not try to make any changes in the repo.

# Solution

ssh into the Nautilus Storage Server: `ssh natasha@172.16.238.15`

(Infra: `https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details`)

`cd /usr/src/kodekloudrepos`

`git clone /opt/beta.git` if you get ‘fatal: could not create work tree dir 'beta': Permission denied’ use sudo: `sudo git clone /opt/beta.git /usr/src/kodekloudrepos/beta`

Check the result: `ls -l`


<img width="847" height="727" alt="image" src="https://github.com/user-attachments/assets/3e42dc6f-da8a-4295-830a-479c8399b3dc" />
