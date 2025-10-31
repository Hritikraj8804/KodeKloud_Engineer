# Instructions

The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in `Stratos DC`. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. `Jump host` is our Ansible controller, and we are going to run Ansible playbooks through `thor` user from `jump host`.

b. There is an inventory file `/home/thor/ansible/inventory` on `jump host`. Using that inventory file test `Ansible ping` from `jump host` to `App Server 1`, make sure ping works.

# Solution

Check inventory file

<img width="827" height="90" alt="image" src="https://github.com/user-attachments/assets/612d9a70-0e35-46ab-a8c9-a6308f802e73" />

Generate SSH key pairs: `ssh-keygen`

This command is used to copy the public key to a remote server, enabling passwordless SSH authentication with the specified user (tony) and host (stapp01): `ssh-copy-id tony@stapp01`

Then change the inventory: `stapp01 ansible_user=tony ansible_ssh_private_key_file=~/.ssh/id_rsa`

And then try to ping: `ansible stapp01 -m ping -i inventory -v` This command runs an Ansible ad-hoc command to ping the stapp01 host. The -m option specifies the module to use, in this case, ping. The -i option is used to specify the inventory file (inventory). The -v option enables verbose mode, providing more detailed output during the execution of the command.

<img width="813" height="351" alt="image" src="https://github.com/user-attachments/assets/56cb3481-67f1-4308-9a7d-49752b2fd889" />
