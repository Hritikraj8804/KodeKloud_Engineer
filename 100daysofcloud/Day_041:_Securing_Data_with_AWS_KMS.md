# Instruction

The Nautilus DevOps team is focusing on improving their data security by using AWS KMS. Your task is to create a KMS key and manage the encryption and decryption of a pre-existing sensitive file using the KMS key.

Specific Requirements:

Create a symmetric KMS key named `nautilus-KMS-Key` to manage encryption and decryption.

Encrypt the provided `SensitiveData.txt` file (located in `/root/`), base64 encode the ciphertext, and save the encrypted version as EncryptedData.bin in the `/root/` directory.

Try to decrypt the same and verify that the decrypted data matches the original file.

Make sure that the KMS key is correctly configured. The validation script will test your configuration by decrypting the `EncryptedData.bin` file using the KMS key you created.

# Solution

### **🛠️ Step 1: Create the KMS Key and Alias**

We will create a symmetric encryption key and assign it the required alias for easy identification.

1. **Create the Key:**
Run this command on your `aws-client` host. It returns a JSON object; copy the **`KeyId`**.
```bash
aws kms create-key --description "nautilus-KMS-Key for sensitive data"
```

![1](https://github.com/user-attachments/assets/539a180e-9192-4683-9f48-207fd156c416)

2. **Create the Alias:**
Replace `<YOUR_KEY_ID>` with the ID from the previous step.
```bash
aws kms create-alias --alias-name alias/nautilus-KMS-Key --target-key-id <YOUR_KEY_ID>
```

![2](https://github.com/user-attachments/assets/8447d665-7b9b-4d35-b97c-1f9df9ba6ab3)

---

### **🔐 Step 2: Encrypt the Sensitive Data**

We will encrypt the `SensitiveData.txt` file and save the output in a binary format as requested.

```bash
# Encrypt, extract the binary ciphertext, and save it
aws kms encrypt \
    --key-id alias/nautilus-KMS-Key \
    --plaintext fileb:///root/SensitiveData.txt \
    --output text \
    --query CiphertextBlob | base64 --decode > /root/EncryptedData.bin
```

![3](https://github.com/user-attachments/assets/4d65c256-620b-4c03-b8cd-8c2867165cbf)

---

### **🔓 Step 3: Decrypt and Verify**

To ensure the encryption worked correctly, we decrypt the binary file back into plaintext and compare it to the original.

1. **Decrypt the file:**
```bash
aws kms decrypt \
    --ciphertext-blob fileb:///root/EncryptedData.bin \
    --output text \
    --query Plaintext | base64 --decode > /root/DecryptedData.txt
```

![4](https://github.com/user-attachments/assets/497aba64-dda0-4c70-bc51-990db25db50c)

2. **Verify the content:**
```bash
diff /root/SensitiveData.txt /root/DecryptedData.txt
```

![5](https://github.com/user-attachments/assets/681db3d9-f6ec-4d09-b601-b4711e67735b)

> If there is no output from the `diff` command, the files are identical, and your task is a success!*

![6](https://github.com/user-attachments/assets/9155ab66-edff-43ea-9e24-579ec6549a41)

👉[Dev.to](https://dev.to/hritikraj8804/aws-141-data-protection-at-rest-mastering-aws-kms-for-file-encryption-217n)
