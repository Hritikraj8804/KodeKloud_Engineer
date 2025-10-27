# Instructions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username `sarah` and password `Sarah_pass123`. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.

Add a slave node named `Storage Server`. It should be labeled as `ststor01` and its remote root directory should be /var/www/html.

We have already cloned repository on Storage Server under `/var/www/html`.

Apache is already installed on all app Servers its running on port 8080.

Create a Jenkins pipeline job named `devops-webapp-job` (it must not be a Multibranch pipeline) and configure it to:

Add a string parameter named BRANCH.
It should conditionally deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.
The pipeline should be conditional, if the value master is passed to the BRANCH parameter then it must deploy the master branch, on the other hand if the value feature is passed to the BRANCH parameter then it must deploy the feature branch.

LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

Open Jenkins, install Plugins

<img width="1245" height="565" alt="image" src="https://github.com/user-attachments/assets/7704c67f-c322-489d-b842-64e2ee6af35f" />

Go back to the terminal:

```bash
[root@ststor01 ~]# cd /var/www
[root@ststor01 www]# ls
html
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 sarah sarah 4096 Aug 29 13:38 html
[root@ststor01 www]# chown -R natasha html/
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha sarah 4096 Aug 29 13:38 html
[root@ststor01 www]# 
```

Go back to the Jenkins add credentials

<img width="1250" height="322" alt="image" src="https://github.com/user-attachments/assets/45e04cd4-0539-44fa-a295-ed0ea2c09e07" />

Create a new Node

<img width="1257" height="546" alt="image" src="https://github.com/user-attachments/assets/17d07c10-380f-4f69-b375-fd5039434543" />

<img width="1240" height="557" alt="image" src="https://github.com/user-attachments/assets/447db107-9d4b-4391-8e9f-311ef63e1d0b" />

<img width="1031" height="552" alt="image" src="https://github.com/user-attachments/assets/89a8996f-67ad-495d-940e-a88515cfc8dc" />

<img width="1250" height="430" alt="image" src="https://github.com/user-attachments/assets/8bc7b5da-ca9b-4e26-bd40-15ef271f4ee3" />

Relaunch agent

<img width="1257" height="473" alt="image" src="https://github.com/user-attachments/assets/b1cea005-61e9-43e0-840b-402231092a41" />

<img width="1241" height="360" alt="image" src="https://github.com/user-attachments/assets/8090a1cd-837d-44c3-9691-d516ce0b8ecb" />

Create a new job, add this script

```python
pipeline {
    agent {
        label 'ststor01'
    }
    
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to deploy (master or feature)')
    }
    
    stages {
        stage('Deploy') {
            when {
                expression {
                    params.BRANCH == 'master' || params.BRANCH == 'feature'
                }
            }
            steps {
                script {
                    def repositoryPath = '/var/www/html/'

                    if (params.BRANCH == 'master') {
                        git branch: 'master',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    } else if (params.BRANCH == 'feature') {
                        git branch: 'feature',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    }
                    
                    sh "cp -r /var/www/html/workspace/devops-webapp-job/* /var/www/html/"
            }
                }
            }
        }
    }

```

Try to build

<img width="1258" height="571" alt="image" src="https://github.com/user-attachments/assets/89c280bb-7025-45ef-8a77-297c241b81f2" />

<img width="1250" height="368" alt="image" src="https://github.com/user-attachments/assets/45f652d4-c43c-4c9c-95a7-7c07d2afa8c7" />

<img width="1216" height="477" alt="image" src="https://github.com/user-attachments/assets/622ee74a-3738-4075-95a2-4756cfbd292a" />

<img width="952" height="201" alt="image" src="https://github.com/user-attachments/assets/21040c5c-83dc-4268-8517-c6e1e786fefe" />

Open Build with Parameters, change Branch to 'feature' and Build

<img width="1256" height="431" alt="image" src="https://github.com/user-attachments/assets/6bce76dd-8be2-4713-bbde-a39f57d19e6f" />

<img width="1092" height="170" alt="image" src="https://github.com/user-attachments/assets/9d27ae4d-a4ae-42cd-9e56-7248b2bb8357" />

Again rebuild with the master

<img width="955" height="202" alt="image" src="https://github.com/user-attachments/assets/b1fb00da-d3e7-499e-9c4e-6fa2e8ad479c" />

Changes come from Gitea

<img width="1248" height="441" alt="image" src="https://github.com/user-attachments/assets/e07171a1-e65b-4f27-a2e4-e9b19664b387" />

<img width="1241" height="432" alt="image" src="https://github.com/user-attachments/assets/54569e03-14db-40e6-8870-eee5fb5dc184" />


