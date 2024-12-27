# Instructions

The Nautilus development team shared requirements with the DevOps team regarding new application development.—specifically, they want to set up
 a Git repository for that project. Create a Git repository on `Storage server` in Stratos DC as per details given below:

1. Install `git` package using `yum` on `Storage server`.
2. After that create a bare repository `/opt/cluster.git` (make sure to use exact name).

# Solution

ssh into the Nautilus Storage Server: `ssh natasha@172.16.238.15`

(Infra: `https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details`)

Run the following command as the root user or with sudo privileges to update the package manager's cache: `sudo yum update`

Once the package manager's cache is updated, you can install git by running the following command: `sudo yum install git`

After the installation is complete, you can verify that git is installed by checking its version: `git --version`
