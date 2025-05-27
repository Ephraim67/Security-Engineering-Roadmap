### **What is an Ansible Playbook?**

An **Ansible playbook** is a **YAML-formatted file** that defines a set of **tasks** for Ansible to execute on **managed nodes**. These tasks are written in a human-readable format and describe the desired state of a system, such as installing software, configuring settings, or restarting services.

#### Key Features:

* Declarative: You state **what** you want, not **how** to do it.
* Idempotent: Running the playbook multiple times won't change the system if it's already in the desired state.
* Scalable: You can target one system or thousands with the same playbook.

#### Example Playbook:

```yaml
---
- name: Install and start Apache web server
  hosts: webservers
  become: yes

  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start Apache service
      service:
        name: httpd
        state: started
        enabled: true
```



### **What is a Node in Ansible?**

In Ansible terminology, a **node** is any machine in your network that Ansible interacts with.

#### There are two types of nodes:

1. **Control Node**:

   * The machine where Ansible is installed.
   * Executes playbooks and sends commands to managed nodes via SSH.

2. **Managed Node**:

   * The remote system(s) you want to configure (e.g., web servers, database servers).
   * Does **not** need Ansible installed.
   * Must be reachable by SSH from the control node.



### **Summary:**

| Term             | Definition                                                             |
| ---------------- | ---------------------------------------------------------------------- |
| **Playbook**     | YAML file containing automation tasks.                                 |
| **Control Node** | Machine running Ansible, controlling operations.                       |
| **Managed Node** | Remote machine that Ansible configures based on playbook instructions. |
