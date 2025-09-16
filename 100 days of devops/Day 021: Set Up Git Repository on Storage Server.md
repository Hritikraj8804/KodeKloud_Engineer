# Tassk
The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:


Utilize yum to install the git package on the Storage Server.



Create a bare repository named /opt/ecommerce.git (ensure exact name usage).

# Soultion-

### 1\. Install Git

First, connect to the **Storage Server** and use the **yum** package manager to install Git. You can do this with the following command:

```bash
sudo yum install git -y
```

The `-y` flag automatically confirms the installation, so you won't be prompted to type 'yes'.

-----

### 2\. Create the Bare Repository

Next, navigate to the `/opt/` directory and create the bare Git repository. A **bare repository** is one that doesn't have a working directory; it's used as a central hub for sharing changes among multiple developers.

Here are the commands to create it with the exact name `/opt/ecommerce.git`:

```bash
cd /opt
sudo git init --bare ecommerce.git
```

The `git init --bare` command initializes an empty Git repository without a working tree. This is the standard practice for a remote repository that will be used for pushing and pulling code.
