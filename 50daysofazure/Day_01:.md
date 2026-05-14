# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create an SSH key pair with the following requirements:

* The name of the SSH key pair should be `nautilus-kp`.
* The key pair type must be `rsa`.

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.

---

# Solution

## **Steps to Complete the Task**

### **1. Open the Integrated Terminal**

Launch the terminal from VS Code.

---

### **2. Navigate to the SSH Directory**

Move to the `.ssh` directory:

```bash id="k3m7xp"
cd ~/.ssh
```

---

### **3. Generate the RSA SSH Key Pair**

Run the following command:

```bash id="u8q2lm"
ssh-keygen -t rsa -f ~/.ssh/nautilus-kp
```

---

### **4. Set a Passphrase (Optional)**

You will see:

```text id="n5v1cz"
Enter passphrase (empty for no passphrase):
```

Press:

```text id="a4w8yt"
Enter
```

to keep it empty, or provide a secure passphrase.

---

### **5. Verify the Generated Keys**

Check whether the key pair was created successfully:

```bash id="r7x9df"
ls -l ~/.ssh/nautilus-kp*
```

Expected output:

```text id="p2m6kb"
~/.ssh/nautilus-kp
~/.ssh/nautilus-kp.pub
```

---

## Files Created

| File Name         | Purpose         |
| ----------------- | --------------- |
| `nautilus-kp`     | Private SSH key |
| `nautilus-kp.pub` | Public SSH key  |

---

## SSH Key Details

| Option    | Value         |
| --------- | ------------- |
| Key Name  | `nautilus-kp` |
| Algorithm | `RSA`         |

---

## Challenges

### **1. Incorrect Key Type**

Ensure the command includes:

```bash id="e1h4qt"
-t rsa
```

Otherwise, another algorithm may be generated.

---

### **2. Wrong File Name**

The filename must exactly match:

```text id="d8s3vn"
nautilus-kp
```

---

### **3. Existing Key Conflict**

If the key already exists, SSH may prompt:

```text id="w6j2mr"
Overwrite (y/n)?
```

Type:

```text id="g5p8la"
y
```

only if replacement is intended.

---

<img width="1072" height="887" alt="keys" src="https://github.com/user-attachments/assets/ac81bc56-de69-4408-afee-df480cccf509" />

<img width="587" height="510" alt="created" src="https://github.com/user-attachments/assets/a5542828-9203-4a42-9101-7334894bcb2a" />

<img width="875" height="337" alt="keysss" src="https://github.com/user-attachments/assets/4ead0334-3487-4170-93ee-976b160208a9" />


## Fun Message

*"One small SSH key for Nautilus, one giant leap toward Azure migration ☁️🔐"*
