# Instructions 

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Create a Jenkins  job named `install-packages` and configure it to accomplish below given tasks.

- Add a string parameter named `PACKAGE`.
- Configure it to install a package on the `storage server` in `Stratos Datacenter` provided to the `$PACKAGE` parameter.

`Note`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`.
 Also some times Jenkins UI gets stuck when Jenkins service restarts in 
the back end so in such case please make sure to refresh the UI page.

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution


Connect to Jenkins, find and install ssh plugins

Create ssh credentials for the storage server user
![1](https://github.com/user-attachments/assets/a3b47ceb-9578-4cc9-bdc8-cf6479cf959e)

Add ssh host in Configure System, check connection, apply and save
![2](https://github.com/user-attachments/assets/8e2ee27f-6d84-4acf-8c39-cbc0b89de53c)

Create a job with these parameters
![3](https://github.com/user-attachments/assets/2b745f28-d348-4fcf-b6b8-7ad7ccc04396)

Configure Build Steps, the command: echo 'Bl@kW' | sudo -S yum install -y $PACKAGE
![4](https://github.com/user-attachments/assets/18b7afa4-e747-4b7f-b357-93d1658f21e6)
