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

Add credentials
![4](https://github.com/user-attachments/assets/da450c0e-fc32-46cd-bbff-f5e71ada2e2e)

Configure ssh remote host
![5](https://github.com/user-attachments/assets/4a78baaf-4496-4392-ae5c-7fc45a8a1b2e)

Test connection
![6](https://github.com/user-attachments/assets/74c4d742-5e20-48cd-a62c-11e4a3dcca03)

Create a job
![7](https://github.com/user-attachments/assets/4d1e171f-9a81-4047-9acf-0f8ed91d1ab7)
![8](https://github.com/user-attachments/assets/2af0924d-eda7-4036-adca-90e415accd32)

Add scp /var/log/httpd/* natasha@ststor01:/usr/src/security
![9](https://github.com/user-attachments/assets/6387920d-ea9d-4989-b31c-629b8a49a523)

Click Build Now
![010](https://github.com/user-attachments/assets/0cfdaf67-522c-44c5-a310-f07664b54040)
![011](https://github.com/user-attachments/assets/c902dd36-499e-463e-a633-5bcfe1c3e6cb)

Check cd /usr/src/security before and after executing the job
![012](https://github.com/user-attachments/assets/b47c9e55-1413-4369-86a7-215c08ad4a41)
