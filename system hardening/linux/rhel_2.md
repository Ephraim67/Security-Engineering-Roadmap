## **Chapter 2: Setting Up Control and Managed Nodes**

Before automating configurations with RHEL system roles, you need to set up two key components:

1. **Control Node** – the system where you run Ansible commands/playbooks.
2. **Managed Nodes** – the systems you want to configure remotely using Ansible.



### **2.1 Preparing the Control Node (RHEL 10)**

#### **What is the Control Node?**

It’s the central system that:

* Runs Ansible
* Connects to and configures managed nodes via SSH
* Applies roles and playbooks defined in your automation setup

#### **Steps to Prepare It:**

1. **System Requirements:**

   * Must be registered with Red Hat Customer Portal
   * Must have a valid RHEL Server subscription

2. **Create an Ansible User:**

   ```bash
   useradd ansible
   su - ansible
   ```

3. **Generate SSH Keys:**

   ```bash
   ssh-keygen
   ```

4. **Configure Ansible Settings (\~/.ansible.cfg):**

   ```ini
   [defaults]
   inventory = /home/ansible/inventory
   remote_user = ansible

   [privilege_escalation]
   become = True
   become_method = sudo
   become_user = root
   become_ask_pass = True
   ```

5. **Create an Inventory File:**
   Example (INI format):

   ```ini
   managed-node-01.example.com

   [US]
   managed-node-02.example.com ansible_host=192.0.2.100
   managed-node-03.example.com
   ```

6. **Install RHEL System Roles:**

   * Without Ansible Automation Platform:

     ```bash
     sudo dnf install rhel-system-roles
     ```
   * With Ansible Automation Platform:

     ```bash
     ansible-galaxy collection install redhat.rhel_system_roles
     ```

---

### **2.2 Preparing Managed Nodes**

#### **What are Managed Nodes?**

These are the remote systems Ansible will configure using playbooks from the control node.

#### **Steps to Prepare Each Managed Node:**

1. **Create the Ansible User:**

   ```bash
   useradd ansible
   passwd ansible
   ```

2. **Copy SSH Public Key from Control Node:**

   ```bash
   ssh-copy-id managed-node-01.example.com
   ```

3. **Verify SSH Connection:**

   ```bash
   ssh managed-node-01.example.com whoami
   ```

4. **Set Up Passwordless or Prompted `sudo` Access:**

   * Use `visudo` to edit:

     ```bash
     visudo /etc/sudoers.d/ansible
     ```
   * Example entries:

     * Prompt for password:

       ```
       ansible ALL=(ALL) ALL
       ```
     * No password prompt:

       ```
       ansible ALL=(ALL) NOPASSWD: ALL
       ```

---

### **Final Verification**

* **Ping All Managed Nodes:**

  ```bash
  ansible all -m ping
  ```

* **Test Privilege Escalation:**

  ```bash
  ansible managed-node-01.example.com -m command -a whoami
  ```

  Should return `root` if sudo is correctly configured.

---

### **Result**

Once these steps are complete:

* Your control node can securely SSH into all managed nodes.
* You can automate configurations across your RHEL systems using system roles and playbooks.
