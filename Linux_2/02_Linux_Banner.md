# Instructions

During the monthly compliance meeting, it was pointed out that several servers in the `Stratos DC`do not have a valid banner. The security team has provided serveral approved templates which should be applied to the servers to maintain compliance. These will be displayed to the user upon a successful login.

Update the `message of the day` on all application and db servers for `Nautilus`. Make use of the approved template located at `/home/thor/nautilus_banner` on jump host

# Solution

scp /home/thor/nautilus_banner tony@stapp01:/tmp/

scp /home/thor/nautilus_banner steve@stapp02:/tmp/

scp /home/thor/nautilus_banner banner@stapp03:/tmp/

scp /home/thor/nautilus_banner peter@stdb01:/tmp/

ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03

ssh peter@stdb01

ssh to each App Server

sudo mv /tmp/nautilus_banner /etc/motd

sudo chmod +x /etc/motd

![1](https://github.com/user-attachments/assets/f8ea5947-c207-4b0c-b960-2933517a9631)
