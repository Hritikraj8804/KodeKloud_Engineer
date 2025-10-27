# Instructions

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Create a Jenkins job named `database-backup`.
2. Configure it to take a database dump of the `kodekloud_db01` database present on the `Database server` in `Stratos Datacenter`, the database user is `kodekloud_roy` and password is `asdfgdsd`.
3. The dump should be named in `db_$(date +%F).sql` format, where `date +%F` is the current date.
4. Copy the `db_$(date +%F).sql` dump to the `Backup Server` under location `/home/clint/db_backups`.
5. Further, schedule this job to run periodically at `*/10 * * * *` (please use this exact schedule format).

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.
2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).
3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

 # Solution
 
Install these plugins

<img width="1211" height="585" alt="image" src="https://github.com/user-attachments/assets/6afd2b96-8582-4d26-bb38-574f4289a5a4" />

Follow the screenshots

<img width="1050" height="572" alt="image" src="https://github.com/user-attachments/assets/ed7b7083-03a0-404b-a791-d3c5d93e9225" />

<img width="1217" height="292" alt="image" src="https://github.com/user-attachments/assets/a7a24670-1462-4d4c-8192-e52db6157690" />

<img width="1043" height="445" alt="image" src="https://github.com/user-attachments/assets/64d1bcb9-d4ba-464d-86ef-ea834afaad0e" />

<img width="1151" height="186" alt="image" src="https://github.com/user-attachments/assets/6520f1e9-e60f-4e3b-b156-fd7df289323e" />

Create a new job

<img width="1205" height="350" alt="image" src="https://github.com/user-attachments/assets/47d65570-50d3-4d40-ab98-99f6b73ac149" />

Enter these commands: 

mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > db_$(date +%F).sql

scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups

<img width="1062" height="396" alt="image" src="https://github.com/user-attachments/assets/055b19aa-56a4-43c3-a901-279c42740440" />

<img width="750" height="357" alt="image" src="https://github.com/user-attachments/assets/eaabaaa2-53d6-4861-866f-d52b46ed7bed" />

ssh peter@172.16.239.10

ssh-keygen -t rsa

ssh-copy-id clint@stbkp01

<img width="975" height="310" alt="image" src="https://github.com/user-attachments/assets/6dd519b6-45f0-482e-b72b-7e1d7aeb9b63" />

ssh clint@stbkp01

<img width="516" height="145" alt="image" src="https://github.com/user-attachments/assets/76f4fc18-1a94-4116-b385-6514a69cfda7" />

Click on Build Now

<img width="1081" height="321" alt="image" src="https://github.com/user-attachments/assets/dc6f5138-228e-4db4-85de-4183681ee965" />

<img width="1037" height="493" alt="image" src="https://github.com/user-attachments/assets/7fd84bb8-aa2b-4e6e-8591-139ae2b65283" />

Check the results again

<img width="637" height="435" alt="image" src="https://github.com/user-attachments/assets/8e9b79f1-f3ba-4949-9851-52e9043d0d49" />
