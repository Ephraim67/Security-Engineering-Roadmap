### **Chapter 3: Ansible Vault — Explained Simply**

This chapter introduces **Ansible Vault**, a powerful tool for **securing sensitive data** in your Ansible projects, such as:

* Passwords
* API keys
* User credentials
* Secret configuration values

#### 🔐 **Why Use Ansible Vault?**

Storing secrets in plain text is a security risk. Ansible Vault **encrypts** your files using **AES256 symmetric encryption**, allowing you to **safely store** sensitive content even in version control (like Git).

---

### 📘 **Core Concepts**

| Concept                   | Description                                                          |
| ------------------------- | -------------------------------------------------------------------- |
| **Vault-encrypted files** | Encrypted files containing sensitive data.                           |
| **Symmetric encryption**  | One password is used for both encrypting and decrypting files.       |
| **Variable usage**        | Vault-protected variables can be used like normal Ansible variables. |

---

### 🔧 **Common Vault Commands**

| Task                            | Command Example                       |
| ------------------------------- | ------------------------------------- |
| Create encrypted file           | `ansible-vault create vault.yml`      |
| View contents of encrypted file | `ansible-vault view vault.yml`        |
| Edit encrypted file             | `ansible-vault edit vault.yml`        |
| Encrypt existing file           | `ansible-vault encrypt plaintext.yml` |
| Decrypt encrypted file          | `ansible-vault decrypt vault.yml`     |
| Change the vault password       | `ansible-vault rekey vault.yml`       |

Each command prompts for a vault password to protect or unlock the data.

---

### 📦 **How to Use Vault in Playbooks**

You reference the encrypted file in your playbook like any other `vars_file`:

```yaml
---
- name: Create user accounts for all servers
  hosts: managed-node-01.example.com
  vars_files:
    - ~/vault.yml

  tasks:
    - name: Create user from vault
      user:
        name: "{{ username }}"
        password: "{{ pwhash }}"
```

#### To Run the Playbook Securely:

```bash
ansible-playbook create-users.yml --ask-vault-pass
```

Or use a password file:

```bash
ansible-playbook create-users.yml --vault-password-file /path/to/passwordfile
```

### ✅ **Best Practices**

* **Keep sensitive variables in separate encrypted files.**
* **Do not commit vault passwords to version control.**
* **Use `--vault-id` for multi-password workflows if needed (advanced use).**


## 🔐 **Step-by-Step: Using Ansible Vault to Secure a Password**



### **Step 1: Create a Hashed Password**

Ansible's `user` module expects **hashed** passwords (not plain text).

Run this on the control node:

```bash
python3 -c "import crypt; print(crypt.crypt('MySecurePassword123', crypt.mksalt(crypt.METHOD_SHA512)))"
```

**Example output:**

```
$6$H72vT5uL$2uiKxTS8L4CgK3JvCBRcTupqJxuxN5T9ZTz5kOXFqipRLYD9KUx.4WVkZlJW7XYeNwUeZX.j6QGxHghr4Q14I1
```



### **Step 2: Create an Encrypted Vault File**

```bash
ansible-vault create vault.yml
```

When prompted, enter and confirm a password for the vault.

In the editor that opens, paste something like:

```yaml
username: myuser
pwhash: "$6$H72vT5uL$2uiKxTS8L4CgK3JvCBRcTupqJxuxN5T9ZTz5kOXFqipRLYD9KUx.4WVkZlJW7XYeNwUeZX.j6QGxHghr4Q14I1"
```

Save and exit.



### **Step 3: Write the Playbook**

Create a file named `create_user.yml`:

```yaml
---
- name: Create a new user securely using Vault
  hosts: all
  become: yes
  vars_files:
    - vault.yml

  tasks:
    - name: Create user
      user:
        name: "{{ username }}"
        password: "{{ pwhash }}"
```



### **Step 4: Run the Playbook**

To run the playbook and get prompted for the vault password:

```bash
ansible-playbook create_user.yml --ask-vault-pass
```

Alternatively, save the vault password to a secure file (e.g., `~/vault-pass.txt`), then run:

```bash
ansible-playbook create_user.yml --vault-password-file ~/vault-pass.txt
```



### ✅ **Validation Tips**

* Ensure SSH works from control node to managed node.
* Use `ansible all -m ping` to confirm inventory is reachable.
* After running the playbook, SSH into the managed node and check if the user exists:

  ```bash
  id myuser
  ```


