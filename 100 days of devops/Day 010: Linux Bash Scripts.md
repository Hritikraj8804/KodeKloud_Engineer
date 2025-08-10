# Linux Backup Script Automation

This document outlines the solution for creating an automated backup script on a Linux server. The primary goal is to back up a website's directory to a local folder and then securely copy it to a remote backup server without manual password entry.

## 🎯 Objective

To create a bash script named `media_backup.sh` on **App Server 2** that:

1.  Creates a zip archive of the `/var/www/html/media` directory.
2.  Saves the archive locally to `/backup/`.
3.  Copies the archive to the **Nautilus Backup Server**.
4.  Executes without requiring a password for the file transfer.
5.  Is runnable by the designated user on App Server 2 (e.g., `steve`).

-----

## Solution Steps

The solution involves three main phases: configuring passwordless SSH, writing the script, and setting the correct permissions.

### 🔑 1. Configure Passwordless SSH

This is the most critical step to enable the script's automation. We will set up an SSH key on the source server (`stapp02`) and copy it to the destination (`stbkp01`).

1.  **Log into App Server 2** (`stapp02`) as the `steve` user.
2.  **Generate an SSH key pair**:
    ```bash
    ssh-keygen
    ```
    *Accept the defaults by pressing Enter for all prompts.*
3.  **Copy the public key to the backup server**:
    ```bash
    ssh-copy-id clint@172.16.238.16
    ```
    *You will be prompted for the `clint` user's password (`H@wk3y3`) one last time.*

### ✍️ 2. Create the Backup Script

Now, create the script file on App Server 2.

1.  **Create the script file**:
    ```bash
    sudo vi /scripts/media_backup.sh
    ```
2.  **Add the following content**:
    ```bash
    #!/bin/bash

    # Create a zip archive of the media directory
    zip -r /backup/xfusioncorp_media.zip /var/www/html/media

    # Copy the archive to the backup server using the passwordless connection
    scp /backup/xfusioncorp_media.zip clint@172.16.238.16:/backup/
    ```

### 🏃 3. Set Permissions

Make the script executable so the `steve` user can run it.

1.  **Make the script executable**:
    ```bash
    sudo chmod +x /scripts/media_backup.sh
    ```
2.  **Assign ownership to the user**:
    ```bash
    sudo chown steve:steve /scripts/media_backup.sh
    ```

-----

## ✅ Verification

After creating and setting permissions for the script, run it:

```bash
/scripts/media_backup.sh
```

To verify that it worked correctly, check that the file exists on the remote backup server. This command should run **without asking for a password**.

```bash
ssh clint@172.16.238.16 "ls -l /backup/"
```

You should see `xfusioncorp_media.zip` listed in the output.
