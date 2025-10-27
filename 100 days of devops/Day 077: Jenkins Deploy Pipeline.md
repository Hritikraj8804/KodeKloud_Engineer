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

<img width="1200" height="568" alt="image" src="https://github.com/user-attachments/assets/2af77eb1-79a5-4a43-801a-6e4e485285c9" />

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

<img width="1220" height="347" alt="image" src="https://github.com/user-attachments/assets/f2dce19c-cf37-49c7-84c8-16764fd8b6a7" />

Create a new Node

<img width="1102" height="473" alt="image" src="https://github.com/user-attachments/assets/fd6ecb8e-373b-442a-815f-dfbbfeb1afeb" />

<img width="1102" height="548" alt="image" src="https://github.com/user-attachments/assets/ea440580-6cd7-4ef0-8470-25d123c90281" />

<img width="1256" height="360" alt="image" src="https://github.com/user-attachments/assets/8f21b103-c786-4c34-9a55-0716f7d317b4" />

Click on Relaunch agent

<img width="1252" height="436" alt="image" src="https://github.com/user-attachments/assets/18065aa8-cf47-4e2e-aa6c-87c4606c2ff9" />

<img width="1240" height="438" alt="image" src="https://github.com/user-attachments/assets/81294453-a8ba-4382-a187-f08830988cbe" />

Create a new job

<img width="1185" height="465" alt="image" src="https://github.com/user-attachments/assets/17a0fda0-f13f-42b4-8d68-9d62a1d30807" />

Add the script

<img width="1240" height="507" alt="image" src="https://github.com/user-attachments/assets/e6578524-576d-4123-8bfe-7768bce661be" />

Build the job

<img width="1056" height="511" alt="image" src="https://github.com/user-attachments/assets/61faa0df-453e-40af-967b-7192a360879b" />

<img width="1223" height="398" alt="image" src="https://github.com/user-attachments/assets/fa96f3f5-152b-4998-b311-9c725a8ad4aa" />

<img width="997" height="262" alt="image" src="https://github.com/user-attachments/assets/39327611-f880-4363-94dd-4937ca80684b" />

These changes come from Gitea

<img width="1250" height="465" alt="image" src="https://github.com/user-attachments/assets/de4b95a2-d5d9-43af-9bf4-841b270145c6" />

