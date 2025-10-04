# TASK
The Nautilus DevOps team possesses confidential data on App Server 3 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.


Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /home/. Ensure the file is not modified during this operation.

# SOLUTION
To copy the encrypted file `/tmp/nautilus.txt.gpg` from the Docker host (App Server 3) to the running container named `ubuntu_latest` at the `/home/` directory, you should use the `docker cp` command.

The `docker cp` command securely copies files between the local filesystem (the Docker host) and a container, **ensuring the file content is not modified** during the transfer.

Here is the command you need to execute on **App Server 3**:

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/
```

### Command Breakdown:

  * **`sudo docker cp`**: Executes the Docker copy command with root privileges (if required).
  * **`/tmp/nautilus.txt.gpg`**: This is the source path on the **Docker Host (App Server 3)**.
  * **`ubuntu_latest:/home/`**: This is the destination, specified as:
      * **`ubuntu_latest`**: The name of the running container.
      * **`:/home/`**: The target path *inside* the container. The file will be placed in `/home/nautilus.txt.gpg` within the container.
   

The most common "check" related to the previous Docker task would be to **verify that the file was successfully copied** to the container.

Please specify what you would like to check:

1.  **Check if the file was copied to the container?**
2.  **Check the status of the Docker service?**
3.  **Check the versions of Docker or Docker Compose?**
4.  **Something else entirely?**

Assuming you want to **verify the file copy** (Scenario 1), here are the steps:

## How to Verify the File Copy

You can use the `docker exec` command to run a command *inside* the running container to list the files in the destination directory.

### Command

Execute the following command on **App Server 3** to list the files in the `/home/` directory of the `ubuntu_latest` container:

```bash
sudo docker exec ubuntu_latest ls -l /home/
```

### Expected Output

If the file copy was successful, you should see `nautilus.txt.gpg` listed in the output, confirming its presence in the `/home/` directory:

```
total 4
-rw-r--r-- 1 root root 1234 Oct  4 09:40 nautilus.txt.gpg
# ... other files/directories in /home/ if any
```
