# Instructions

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. 
Therefore, perform the steps below:

a. Install `cronie` package on all `Nautilus` app servers and start `crond` service.

b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

# Solution

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

Run the appropriate command based on the package manager used by your servers:`sudo yum install cronie`

After installing the `cronie` package, start the `crond` service: `sudo systemctl start crond`

To check if the `crond` service is running on a Nautilus app server, you can use the following command: `sudo systemctl status crond`

![1](https://github.com/user-attachments/assets/0c30c6fb-2d32-4e37-aeb9-764e8bca7734)


Open the crontab editor for the root user by executing the following command: `sudo crontab -e -u root`

In the crontab editor, add the following line: `*/5 * * * * echo hello > /tmp/cron_text`

Then open /tmp/ after 5min the file will appear there

![2](https://github.com/user-attachments/assets/38b696ce-39c3-4a6e-ab08-5efc3d8a4d7f)
