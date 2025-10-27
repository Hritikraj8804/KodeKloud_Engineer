# Instructions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username `sarah` and password `Sarah_pass123`. There under user sarah you will find a repository named web_app that is already cloned on Storage server under `/var/www/html`. sarah is a developer who is working on this repository.


Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.

1. We have already cloned repository on `Storage Server` under `/var/www/html`.

2. Apache is already installed on all app Servers its running on port 8080.

3. Create a Jenkins pipeline job named `nautilus-webapp-job` (it must not be a Multibranch pipeline) and configure it to:

- Deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.

- LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.

Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

`ssh natasha@ststor01`

`sudo su -`

`yum install java-17-openjdk -y`

Open Jenkins, install these plugins



Go back to the terminal:

```bash
[root@ststor01 ~]# cd /var/www
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 sarah sarah 4096 Aug 30 16:38 html
[root@ststor01 www]# chown -R natasha html/
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha sarah 4096 Aug 30 16:38 html
[root@ststor01 www]#
```

Create credentials



Create a new Node


Click on Relaunch agent



Create a new job



Add the script



Build the job



These changes come from Gitea

![image](https://github.com/user-attachments/assets/9ae64eee-7a1e-4c42-9163-e14630b74576)
