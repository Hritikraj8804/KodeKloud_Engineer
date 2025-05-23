# Instructions

Some developers are working on a common repository where they are testing some features for an application. They are having three branches (excluding the master branch) in this repository where they are adding changes related to these different features. They want to test these changes on Stratos DC app servers so they need a Jenkins job using which they can deploy these different branches as per requirements. Configure a Jenkins job accordingly.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on `Gitea` button to access the Gitea page. Login to `Gitea` server using username `sarah` and password `Sarah_pass123`.

1. There is a Git repository named `web_app` on Gitea where developers are pushing their changes. It has three branches `version1`, `version2` and `version3` (excluding the `master` branch). You need not to make any changes in the repository.
2. Create a Jenkins job named `app-job`.
3. Configure this job to have a choice parameter named `Branch` with choices as given below:

`version1`

`version2`

`version3`

1. Configure the job to fetch changes from above mentioned Git repository and make sure it should fetches the changes from the respective branch which you are passing as a choice in the choice parameter while building the job. For example if you choose `version1` then it must fetch and deploy the changes from branch `version1`.
2. Configure this job to use custom workspace rather than a default workspace and custom workspace directory should be created under `/var/lib/jenkins` (for example `/var/lib/jenkins/version1`) location rather than under any sub-directory etc.  The job  should use a workspace as per the value you will pass for `Branch` parameter while building the job. For example if you choose `version1` while building the job then it should create a workspace directory called `version1` and should fetch Git repository etc within that directory only.
3. Configure the job to deploy code (fetched from Git repository) on storage server (in Stratos DC) under `/var/www/html` directory. Since its a shared volume.
4. You can access the website by clicking on `App` button.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.
2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

# Solution

This is the only solution that worked for me

Install Gitea and Publish Over SSH plugins
![01](https://github.com/user-attachments/assets/9b0cb62c-e167-48e0-a7f7-944b00e5b432)

Then just follow the screenshots
![02](https://github.com/user-attachments/assets/1f3d99f3-8896-403c-887a-7972e8f1557c)
![03](https://github.com/user-attachments/assets/f273b4fe-ef1f-47fb-9d08-9021af9c520e)

Add under Publish over SSH
![04](https://github.com/user-attachments/assets/3261ba7c-82a2-4a5d-82c8-169bdc45847b)

Test configuration
![05](https://github.com/user-attachments/assets/c213947a-65f2-4f02-b252-fe5ce8b0c988)


ssh into the Storage Server: `ssh natasha@172.16.238.15`
![06](https://github.com/user-attachments/assets/7f2526c3-52bd-4acf-a8b3-0ef8093ab25e)

Create a job
![07](https://github.com/user-attachments/assets/63eab184-f621-43c5-889d-c674cec677b5)
![08](https://github.com/user-attachments/assets/acb28855-46aa-4144-8255-cfd00537f0b9)
![09](https://github.com/user-attachments/assets/ebea5262-c3d3-4319-8472-f0c219c67b75)

Copy the link
![10](https://github.com/user-attachments/assets/958435f7-8bee-4496-9b17-bcda73bb2495)
![11](https://github.com/user-attachments/assets/c0c04e28-dffb-442d-9c76-0e621347b4ec)
![12](https://github.com/user-attachments/assets/f35bcc1b-b801-4f7a-8971-4a779f70e562)

Run the job, select 'version 1'. You should see `SSH: Transferred 1 file(s)`
![13](https://github.com/user-attachments/assets/ffaaa4c3-de3d-4079-b637-31fe9312896e)

Open the App link
![14](https://github.com/user-attachments/assets/36b01e4e-2f5d-49c4-94a2-dce8976e16bf)
