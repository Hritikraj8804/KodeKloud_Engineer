# Instructions

The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`

1. Create a Jenkins jobs named `copy-logs`.

2. Configure it to periodically build `every 9 minutes` to copy the Apache logs (`both access_log and error_logs`) from `App Server 3` (`from default logs location`)  to location `/usr/src/security` on `Storage Server`.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in 
the back end. In this case please make sure to refresh the UI page.

2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution

Install these plugins
![1](https://github.com/user-attachments/assets/ca2a0aa1-5678-4e72-be71-e28031caaf96)

ssh to App Server 3 and check logs location

ssh banner@stapp03

sudo su

cd /var/

cd log

cd httpd
![2](https://github.com/user-attachments/assets/2fb0d246-9a56-4029-a81e-564e8f0e68e0)

[banner@stapp03 ~]$ ssh-keygen -t rsa

cd .ssh/

ssh-copy-id natasha@ststor01
![3](https://github.com/user-attachments/assets/1e27012e-e8a0-4591-b7f4-8c6789153828)
