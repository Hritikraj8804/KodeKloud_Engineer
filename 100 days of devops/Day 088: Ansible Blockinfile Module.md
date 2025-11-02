# Instructions

The Nautilus DevOps team wants to install and set up a simple `httpd` web server on all app servers in `Stratos DC`.
 Additionally, they want to deploy a sample web page for now using 
Ansible only. Therefore, write the required playbook to complete this 
task. Find more details about the task below.

We already have an `inventory` file under `/home/thor/ansible` directory on `jump host`. Create a `playbook.yml` under `/home/thor/ansible` directory on `jump host` itself.

1. Using the playbook, install `httpd` web server on all app servers. Additionally, make sure its service  should up and running.
2. Using `blockinfile` Ansible module add some content in `/var/www/html/index.html` file. Below is the content:

`Welcome to XfusionCorp!`

`This is  Nautilus sample file, created using Ansible!`

`Please do not modify this file manually!`

1. The `/var/www/html/index.html` file's user and group `owner` should be `apache` on all app servers.
2. The `/var/www/html/index.html` file's permissions should be `0644` on all app servers.

`Note:`

i. Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.

ii. Do not use any custom or empty `marker` for `blockinfile` module.

# Solution

Check inventory file

```
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

Create a playbook

```YAML
---
- name: Install and configure Apache web server
  hosts: stapp01, stapp02, stapp03
  become: true

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Start httpd service
      service:
        name: httpd
        state: started
        enabled: true

    - name: Add content to index.html
      blockinfile:
        path: /var/www/html/index.html
        block: |
          Welcome to XfusionCorp!

          This is Nautilus sample file, created using Ansible!

          Please do not modify this file manually!
        create: yes

    - name: Set file owner and group
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache

    - name: Set file permissions
      file:
        path: /var/www/html/index.html
        mode: "0644"
```

<img width="922" height="632" alt="image" src="https://github.com/user-attachments/assets/9bb91fb9-9d7f-4860-ab8a-59ec7fc0d95a" />

Check the result: `ansible all -a 'ls -l /var/www/html/' -i inventory`

<img width="491" height="516" alt="image" src="https://github.com/user-attachments/assets/88831b1e-cd9f-43c7-a699-c4b91e9f94a8" />
