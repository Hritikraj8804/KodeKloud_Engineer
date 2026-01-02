# Instruction

The Nautilus DevOps team needs to set up a new EC2 instance that can be accessed securely from their landing host (aws-client). The instance should be of type `t2.micro` and named `devops-ec2`. A new SSH key should be created on the `aws-client` host under the `/root/.ssh/` folder, if it doesn't already exist. This key should then be added to the root user's authorised keys on the EC2 instance, allowing passwordless SSH access from the `aws-client` host.

# Solution

We’ll follow a logical workflow from the client machine to AWS Cloud.

---

### Phase A: Generate the SSH Key Pair

1. **Navigate to the `.ssh` folder:**

```bash
cd /root/.ssh/
```

2. **Generate a new RSA key pair (if it doesn’t already exist):**

```bash
ssh-keygen -t rsa
```

  ![0 1](https://github.com/user-attachments/assets/74efafbf-8468-4689-bcc3-9ea29e4ab18a)


3. **Copy the public key** (`.pub` file) for later use. This is the key that will allow passwordless SSH login.

  ![0 2](https://github.com/user-attachments/assets/d1335e93-2c0a-4552-b416-a264285a53d8)


---

### Phase B: Launch EC2 Instance with Automation

1. **Configure the instance:**

   * **Name:** `devops-ec2`
   * **Instance Type:** `t2.micro`
   * **AMI:** Ubuntu or Amazon Linux (choose according to preference).
     ![4](https://github.com/user-attachments/assets/860a8cb7-8b0f-4f31-b64d-ec543a6d7606)

     ![5](https://github.com/user-attachments/assets/a3bbaf47-6bff-45cb-bbe1-dfbbaea461cb)


2. **Add User Data script** in the **Advanced Details** section during instance launch:

```bash
#!/bin/bash
# Prepare root SSH access
mkdir -p /root/.ssh
chmod 700 /root/.ssh

# Add your public key (replace with actual key)
echo "ssh-rsa YOUR_COPIED_PUBLIC_KEY_HERE" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys

# Enable root login via SSH
sed -i 's/^#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
systemctl restart sshd
```

This ensures that your root user is ready for passwordless SSH access as soon as the instance starts.

---

### Phase C: Optional Manual Hardening

Some AMIs may require manual adjustments:

1. **Log in as default user** (`ec2-user` or `ubuntu`).
2. **Switch to root:**

```bash
sudo su -
```

3. **Verify SSH root login settings in `/etc/ssh/sshd_config`:**

```
PermitRootLogin yes
```

4. **Restart SSH service:**

```bash
systemctl restart sshd
```

---

### Phase D: Verify Passwordless SSH

1. **Test connection from `aws-client`:**

```bash
ssh root@<EC2_PUBLIC_IP>
```

2. **Confirm:** If you reach the root prompt without a password, the setup is complete. 🎉
  ![res](https://github.com/user-attachments/assets/b5c89a3f-871f-4cc4-a00b-7232192ad9e1)

---

## 3. Key Takeaways 📝

* **User Data Scripts:** Automate instance configuration during launch.
* **SSH Permissions Matter:** `.ssh` must be `700` and `authorized_keys` `600`.
* **Root Access:** Enabled here for simplicity, but in production, use a dedicated user with `sudo` privileges.

---

## 4. Common Mistakes to Avoid 🚫

* **Incorrect Path:** Add the key to `/root/.ssh/`, not the default user folder.
* **Typos in Commands:** Errors in `sed` or permissions commands can lock you out.
* **Public vs Private Keys:** Only upload the public key to the server. Keep the private key secure.

---

👉[Dev.to](https://dev.to/hritikraj8804/aws-122-passwordless-ssh-automation-with-ec2-user-data-3pam)
