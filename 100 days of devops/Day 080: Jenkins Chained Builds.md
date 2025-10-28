# Instructions

The DevOps team was looking for a  solution where they want to restart Apache service on all app servers  if the deployment goes fine on these servers in Stratos Datacenter.  After having a discussion, they came up with a solution to use Jenkins  chained builds so that they can use a downstream job for services which  should only be triggered by the deployment job. So as per the  requirements mentioned below configure the required Jenkins jobs.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Similarly you can access Gitea UI on port 8090 and username and password for Git is `sarah` and `Sarah_pass123` respectively. Under user sarah you will find a repository named web.

Apache is already installed and configured on all app server so no changes are needed there. The doc root `/var/www/html` on all these app servers is shared among the Storage server under `/var/www/html` directory.

1. Create a Jenkins job named `nautilus-app-deployment` and configure it to pull change from the master branch of web repository on Storage server under /var/www/html directory, which is already a local git repository tracking the origin web repository. Since /var/www/html on Storage server is a shared volume so changes should auto reflect on all apps.

2. Create another Jenkins job named `manage-services` and make it a downstream job for nautilus-app-deployment job. Things to take care about this job are:

a. This job should restart httpd service on all app servers.
b. Trigger this job only if the upstream job i.e `nautilus-app-deployment` is stable.

LB server is already configured. Click on the App  button on the top bar to access the app. You should be able to see the  latest changes you made. Please make sure the required content is  loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like  https://<LBR-URL>/web etc.

Note: 

 1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre.  Also some times Jenkins UI gets stuck when Jenkins service restarts in  the back end so in such case please make sure to refresh the UI page.

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.

3. Deployment related tasks should be done by sudo user on the destination server to avoid any permission issues so make sure to configure your Jenkins job accordingly.

4. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

Install these Jenkins plugins: SSH, SSH Credentials, SSH Build Agents, Git, Credentials, Pipeline, Publish over SSH

Create Credentials

<img width="1212" height="421" alt="image" src="https://github.com/user-attachments/assets/ad07dc55-b497-450b-a3f7-ee2b16351c77" />

Connect to the Storage server

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

Create/Relaunch Node

<img width="1246" height="312" alt="image" src="https://github.com/user-attachments/assets/bc10b4ea-7636-4bde-aa0a-8d777860bcd5" />

Check `/var/www/html`

```bash
[root@ststor01 ~]# cd /var/www/html
[root@ststor01 html]# ls
index.html
[root@ststor01 html]# cat index.html 
Welcome
[root@ststor01 html]#
```

Check Gitea index file

<img width="1172" height="360" alt="image" src="https://github.com/user-attachments/assets/af092d5e-bc32-46ba-b2aa-3af14723153e" />

Add SSH remote hosts to the System for each App

<img width="1235" height="597" alt="image" src="https://github.com/user-attachments/assets/8fedbd1b-cb5e-4a67-9237-6f81b7b6e1ab" />

Add SSH server (Publish Over SSH)

<img width="1247" height="582" alt="image" src="https://github.com/user-attachments/assets/6621e7df-b5a0-4c9f-a18b-05ab562a4fc5" />

Create `nautilus-app-deployment` job

```bash
pipeline {
    agent {
        label 'ststor01'
    }

    stages {
        stage('Checkout') {
            steps {
                // Connect to GIT and download the repo code
                git credentialsId: 'sarah', url: 'http://git.stratos.xfusioncorp.com/sarah/web.git'
            }
        }
        
        stage('Update Code') { // Or 'Sync with Git' or 'Pull Changes'
            steps {
                // Pull latest changes from the Git repository
                sh 'cd /var/www/html && git pull origin master'
            }
        }
    }
}

```

<img width="1115" height="447" alt="image" src="https://github.com/user-attachments/assets/788fd84a-e3b6-4f54-b19a-d3f01f92c5ba" />

Run it (these are the final logs together with `Triggering a new build of manage-services`)

```bash
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on ststor01 in /var/www/html/workspace/nautilus-app-deployment
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] git
The recommended git tool is: NONE
using credential sarah
Fetching changes from the remote Git repository
Checking out Revision 95386b9b89b6b747ab02fc3774b09684093c2e13 (refs/remotes/origin/master)
Commit message: "Updated index.html file"
 > git rev-parse --resolve-git-dir /var/www/html/workspace/nautilus-app-deployment/.git # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web.git # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web.git
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 95386b9b89b6b747ab02fc3774b09684093c2e13 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master 95386b9b89b6b747ab02fc3774b09684093c2e13 # timeout=10
 > git rev-list --no-walk 95386b9b89b6b747ab02fc3774b09684093c2e13 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Update Code)
[Pipeline] sh
+ cd /var/www/html
+ git pull origin master
From http://git.stratos.xfusioncorp.com/sarah/web
 * branch            master     -> FETCH_HEAD
Already up to date.
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Triggering a new build of manage-services #2
Finished: SUCCESS
```

Before and after running the job

```bash
[root@ststor01 html]# cat index.html 
Welcome
[root@ststor01 html]# cat index.html 
Welcome to KodeKloud!
```

Also check Apps

```bash
[root@ststor01 ~]# cd /var/www/html
[root@ststor01 html]# ls
caches  index.html  remoting  remoting.jar  workspace
[root@ststor01 html]# cat index.html 
Welcome to KodeKloud!
[root@ststor01 html]#
```

Then create a new job `manage-service` as a freestyle project

<img width="1117" height="541" alt="image" src="https://github.com/user-attachments/assets/dabb98c6-4846-400c-bbab-522f5632f465" />

Add this for all Apps

<img width="1247" height="592" alt="image" src="https://github.com/user-attachments/assets/5fb91789-bcd7-4e1c-83ad-9740f7a925b0" />

Run it, here are the final logs

```bash
Started by upstream project "nautilus-app-deployment" build number 3
originally caused by:
 Started by user admin
Running as SYSTEM
Building on the built-in node in workspace /var/lib/jenkins/workspace/manage-services
[SSH] script:

echo Am3ric@ | sudo -S systemctl restart httpd

[SSH] executing...
[sudo] password for steve: 
[SSH] completed
[SSH] exit-status: 0

[SSH] script:

echo Ir0nM@n | sudo -S systemctl restart httpd

[SSH] executing...
[sudo] password for tony: 
[SSH] completed
[SSH] exit-status: 0

[SSH] script:

echo BigGr33n | sudo -S systemctl restart httpd

[SSH] executing...
[sudo] password for banner: 
[SSH] completed
[SSH] exit-status: 0

Finished: SUCCESS
```

Double check Apps `sudo systemctl status httpd`

<img width="1197" height="540" alt="image" src="https://github.com/user-attachments/assets/0527e5ef-0865-4374-8a28-924e9d82e4d2" />

Open App

<img width="1120" height="162" alt="image" src="https://github.com/user-attachments/assets/7dc3dcae-b5e2-49e8-a041-b33f3c29bdd8" />
