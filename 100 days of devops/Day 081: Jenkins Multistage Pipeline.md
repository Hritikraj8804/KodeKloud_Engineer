# Instructions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.

There is a repository named sarah/web in Gitea that is already cloned on Storage server under /var/www/html directory.

Update the content of the file index.html under the same repository to Welcome to xFusionCorp Industries and push the changes to the origin into the master branch.

Apache is already installed on all app Servers its running on port 8080.

Create a Jenkins pipeline job named deploy-job (it must not be a Multibranch pipeline job) and pipeline should have two stages Deploy and Test ( names are case sensitive ). Configure these stages as per details mentioned below.

a. The Deploy stage should deploy the code from web repository under /var/www/html on the Storage Server, as this location is already mounted to the document root /var/www/html of all app servers.

b. The Test stage should just test if the app is working fine and website is accessible. Its up to you how you design this stage to test it out, you can simply add a curl command as well to run a curl against the LBR URL (http://stlb01:8091) to see if the website is working or not. Make sure this stage fails in case the website/app is not working or if the Deploy stage fails.

Click on the App button on the top bar to see the latest changes you deployed. Please make sure the required content is loading on the main URL http://stlb01:8091 i.e there should not be a sub-directory like http://stlb01:8091/web etc.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

## First,  updat manually index.html in Gitea

![gittea](https://github.com/user-attachments/assets/6313dc53-babd-4b86-9829-146c34047a05)

 install these Plugins in Jenkins:
1. ssh
2. Git plugin
3. Pipeline

## connect to ststor01

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

```bash
[root@ststor01 ~]# cd /var/www/html
[root@ststor01 html]# ls
index.html
[root@ststor01 html]# ls -la
total 16
drwxr-xr-x 3 natasha natasha 4096 Sep  5 09:01 .
drwxr-xr-x 3 natasha natasha 4096 Sep  5 08:59 ..
drwxr-xr-x 8 natasha natasha 4096 Sep  5 09:01 .git
-rw-r--r-- 1 natasha natasha    8 Sep  5 09:01 index.html
[root@ststor01 html]# cat index.html 
Welcome
[root@ststor01 html]# cat index.html   #check after you run the job in Jenkins
Welcome to xFusionCorp Industries
[root@ststor01 html]# 
```

## create credentials


 create a new Node and relaunch it
1. **Go to Jenkins → Manage Jenkins → Manage Nodes and Clouds → New Node**.
2. Enter:
   - **Node name:** `Storage server` (or any name you prefer)
   - Select **Permanent Agent** → Click **OK**.
3. Configure the node:
   - **Remote root directory:** `/var/www/html` (this is where the workspace will live)
   - **Labels:** `Storage server` (important — must match `agent { label 'Storage server' }` in pipeline)
   - **Launch method:** 
     - If using SSH: select **Launch agents via SSH**
       - **Host:** IP or hostname of Storage server
       - **Credentials:** username/password for the Storage server
     - Or **via JNLP** if you want the agent to connect itself.
   - Leave other defaults unless required.
4. Click **Save**.
5. Make sure the node is **online** (green dot).

![node_ruuning](https://github.com/user-attachments/assets/cd9a5d0f-c4fd-4ce6-9a25-b123917aa951)

## create a job:
1. Go to Jenkins Dashboard → New Item.

2. Name the job: `deploy-job` (must not be Multibranch).

3. Choose Pipeline and click OK.

4. Scroll down to Pipeline section → Definition → Pipeline script.

5. Enter the pipeline script:

 ```bash
pipeline {
    agent { label 'Storage server' }
    stages {
        stage('Deploy') {
            steps {
                git url: 'http://git.stratos.xfusioncorp.com/sarah/web.git', credentialsId: 'GIT_CREDS', branch: 'master'
                sh '''
                cd /var/www/html
                git pull origin master
                '''
            }
        }
        stage('Test') {
            steps {
                script {
                    def urls = ['http://stapp01:8080', 'http://stapp02:8080', 'http://stapp03:8080', 'http://stlb01:8091']
                    urls.each { url ->
                        sh "curl -s ${url} | grep -F 'Welcome to xFusionCorp Industries'"
                    }
                }
            }
        }
    }
}

```


Here are the logs

```bash
Started by user admin

[Pipeline] Start of Pipeline
[Pipeline] node
Running on Storage server
 in /var/www/html/workspace/deploy-job
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] git
The recommended git tool is: NONE
using credential GIT_CREDS
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web.git
 > git init /var/www/html/workspace/deploy-job # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web.git
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision 02331215973f945d2025656f981f757c99219bf6 (refs/remotes/origin/master)
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 02331215973f945d2025656f981f757c99219bf6 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b master 02331215973f945d2025656f981f757c99219bf6 # timeout=10
Commit message: "Update 'index.html'"
First time build. Skipping changelog.
[Pipeline] sh
+ cd /var/www/html
+ git pull origin master
From http://git.stratos.xfusioncorp.com/sarah/web
 * branch            master     -> FETCH_HEAD
   42bf207..0233121  master     -> origin/master
Updating 42bf207..0233121
Fast-forward
 index.html | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ curl -s http://stapp01:8080
+ grep -F 'Welcome to xFusionCorp Industries'
Welcome to xFusionCorp Industries
[Pipeline] sh
+ curl -s http://stapp02:8080
+ grep -F 'Welcome to xFusionCorp Industries'
Welcome to xFusionCorp Industries
[Pipeline] sh
+ curl -s http://stapp03:8080
+ grep -F 'Welcome to xFusionCorp Industries'
Welcome to xFusionCorp Industries
[Pipeline] sh
+ curl -s http://stlb01:8091
+ grep -F 'Welcome to xFusionCorp Industries'
Welcome to xFusionCorp Industries
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

## Check on each app:
restart on each app: `sudo systemctl restart httpd`

```bash
[root@stapp01 ~]# cd /var/www/html
[root@stapp01 html]# ls
caches  index.html  remoting  remoting.jar  workspace
[root@stapp01 html]# cat index.html 
Welcome to xFusionCorp Industries
[root@stapp01 html]#
[root@stapp01 html]# curl http://stapp01:8080
Welcome to xFusionCorp Industries
[root@stapp01 html]# 
[root@stapp01 html]# curl http://stlb01:8091
Welcome to xFusionCorp Industries
[root@stapp01 html]# 
```
## Check the App

![app_2](https://github.com/user-attachments/assets/088fcf06-9527-49c5-b0ab-ff26f611a251)
