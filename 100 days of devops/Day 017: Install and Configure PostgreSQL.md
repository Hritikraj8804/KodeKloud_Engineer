# Task

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:





PostgreSQL database server is already installed on the Nautilus database server.



a. Create a database user kodekloud_cap and set its password to Rc5C9EyvbU.



b. Create a database kodekloud_db10 and grant full permissions to user kodekloud_cap on this database.



Note: Please do not try to restart PostgreSQL server service.

# Solution

### \#\# 1. Connect to the Database Server

First, connect to the Nautilus database server and switch to the `postgres` user to gain administrative access to the database.

1.  **SSH into the database server** (`stdb01`):
    ```bash
    ssh peter@172.16.239.10
    ```
2.  **Switch to the `postgres` user**:
    ```bash
    sudo -i -u postgres
    ```

### \#\# 2. Create the User and Database

Now, open the PostgreSQL interactive terminal (`psql`) and run the SQL commands.

1.  **Start the `psql` terminal**:

    ```bash
    psql
    ```

2.  **Execute the following SQL commands**. Remember to end each command with a semicolon (**`;`**).

      * **Create the database user**:
        ```sql
        CREATE USER kodekloud_cap WITH PASSWORD 'Rc5C9EyvbU';
        ```
      * **Create the database**:
        ```sql
        CREATE DATABASE kodekloud_db10;
        ```
      * **Grant all permissions to the user on the new database**:
        ```sql
        GRANT ALL PRIVILEGES ON DATABASE kodekloud_db10 TO kodekloud_cap;
        ```

### \#\# 3. Verification and Exit (Optional)

You can verify that the user and database were created correctly.

1.  **List users** to see `kodekloud_cap`:
    ```sql
    \du
    ```
2.  **List databases** to see `kodekloud_db10` and its owner:
    ```sql
    \l
    ```
3.  **Exit the `psql` terminal** and the `postgres` user session:
    ```sql
    \q
    exit
    ```

The task is now complete.
