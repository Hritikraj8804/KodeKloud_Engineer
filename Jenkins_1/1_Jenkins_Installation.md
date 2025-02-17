# Instructions

The DevOps team of xFusionCorp Industries is planning to setup some CI/CD pipelines. After several meetings they have decided to use Jenkins
 server. So, we need to setup a `Jenkins Server` as soon as possible. Please complete the task as per requirements mentioned below:

1. Install `jenkins` on jenkins server using `yum` utility only, and start its `service`. You might face timeout issue while starting the Jenkins service, please refer [this](https://www.jenkins.io/doc/book/system-administration/systemd-services/#starting-services) link for help.

2. Jenkin's admin user name should be `theadmin`, password should be `Adm!n321`, full name should be `Ammar` and email should be `ammar@jenkins.stratos.xfusioncorp.com`.

`Note:`

1. For this task, ssh into the `jenkins` server using user `root` and password `S3curePass` from `jump host`.

2. After installing the Jenkins server, please click on the `Jenkins` button on the top bar to access Jenkins UI and follow the on-screen instructions to create an admin user.

# Solution

ssh into the Jenkins server: `ssh root@jenkins`

This command uses the yum package manager to install the wget utility. wget is a command-line tool used for downloading files from the web: `yum install wget -y`

This command uses wget to download the Jenkins repository configuration file and save it as /etc/yum.repos.d/jenkins.repo:
`wget -O /etc/yum.repos.d/jenkins.repo \

This command lists the files in the /etc/yum.repos.d/ directory. It is used to verify that the jenkins.repo file was successfully downloaded and saved in the correct location: `ls -l /etc/yum.repos.d/`

This command imports the GPG key for the Jenkins repository. GPG keys are used to verify the authenticity and integrity of software packages. Importing the key ensures that the packages obtained from the Jenkins repository are trusted.

`rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key`

This command uses yum to install the Jenkins package: `yum install jenkins`
