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
![1](https://github.com/user-attachments/assets/cb24c700-a035-4b07-9486-bf0444b78307)

Follow the screenshots
![2](https://github.com/user-attachments/assets/7f850d5a-6da0-4fb7-aeba-d06f8c806567)
![3](https://github.com/user-attachments/assets/1b4ccbd9-1250-47d7-bf83-d4d1306c8f5f)
![4](https://github.com/user-attachments/assets/1ad0ef26-bc34-4e1f-8f96-bd6cb4167113)
![5](https://github.com/user-attachments/assets/8256097e-9721-4da5-80ed-c42871442e51)

Create a new job
![6](https://github.com/user-attachments/assets/b15d2075-3126-4e23-a253-0b2e5119d16c)

Enter these commands: 

mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > db_$(date +%F).sql

scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups
![7](https://github.com/user-attachments/assets/00d6ed07-db51-4348-8e40-0ea2c1653efe)
![8](https://github.com/user-attachments/assets/103f344f-f206-4777-b721-6dbd28bc7420)

ssh peter@172.16.239.10

ssh-keygen -t rsa

ssh-copy-id clint@stbkp01
![11](https://github.com/user-attachments/assets/ad78a78d-f54a-4f7a-be19-1e8975b3e4c3)

ssh clint@stbkp01
![12](https://github.com/user-attachments/assets/a438f3f5-d4dd-4c34-9bf8-1ad4b2c61c8d)

Click on Build Now
![13](https://github.com/user-attachments/assets/af929338-681a-4c24-b856-71563d1e12a4)
![14](https://github.com/user-attachments/assets/3a250c18-7dc8-49da-b495-500e455fe340)

Check the results again
![15](https://github.com/user-attachments/assets/3826d43e-3701-4e3a-bbd8-0709597d7b6e)
