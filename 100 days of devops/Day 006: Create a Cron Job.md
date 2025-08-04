## CRON job   
#### The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below: 
 #### 1. Install cronie package on all Nautilus app servers and start crond service. 
 #### 1. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.
### Click on ✔ and Do The Task Again
#### Solution:
### Open Three Terminal and ssh on all App server
#### on Terminal One
 ```
 ssh tony@stapp01
 ```
#### on Terminal Two
 ```
 ssh steve@stapp02
 ```
#### on Terminal Three
 ```
 ssh banner@stapp03
 ```
### Do below Steps on each App Server
#### Install cronie in the server :
 ```
 sudo yum install -y cronie
 ```
#### Start cron service: 
 ```
 sudo systemctl start crond.service
 ``` 
#### Check cron service status: 
 ```
 sudo systemctl status crond.service
 ```
#### Create a cron job by typing : 
 ```
 sudo crontab -e
 ```
#### Write the cron job:
 ```
 */5 * * * * echo hello  >  /tmp/cron_text
 ```
#### Check cron job: 
 ```
 sudo crontab -l
 ```
#### Check cron job for root: 
 ```
 sudo crontab -u root -l
 ```
