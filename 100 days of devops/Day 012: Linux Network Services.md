Linux Network Services	
##### Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 8081 (which is our Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue. Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.
#### Click on ✔ and Do Task Again
##### Solution:-
#### At first Login to stapp01 server or the server that has issue
```
ssh tony@stapp01
```
#### Login as root:
```sudo su```
#### Then check httpd 
systemctl status httpd
#### If the status is inactive then find the error by typing
```httpd -t```
#### Then go to httpd.conf by typing 
```vi /etc/httpd/conf/httpd.conf```
#### Uncomment ServerName
```ServerName 172.16.238.10:8081 ## SetPort According to Task```
##### Save it by :wq!
##### Next check which service is blocking your port by typing this command
```netstat -anp |grep LISTEN |grep ":8081"```
#### Kill the process 
```kill -9 450   # 450 pid_number According to Above Command```
#### Then restart httpd
```
systemctl restart httpd
systemctl status httpd
```
#### Next add rules to the iptables And Change 8081 According To Task
```
sudo iptables -I INPUT -p tcp -m tcp --dport 8081 -j ACCEPT  && sudo iptables-save > /etc/sysconfig/iptables &&  cat /etc/sysconfig/iptables
```
#### Now check from Jump Server
```
curl http://172.16.238.10:8081
```
