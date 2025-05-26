### **Overview**

RHEL system roles are a set of **Ansible roles** developed by Red Hat that allow you to **automate and standardize system administration tasks** across multiple RHEL systems—even across different major versions (e.g., RHEL 7 through 10). This ensures consistency, scalability, and efficiency in managing large environments.



### **Key Concepts in the Ansible Environment**

These are foundational elements you need to understand when working with RHEL system roles:

* **Control Node**
  The system from which Ansible playbooks are run. It can be:

  * A RHEL system (version 7 to 10)
  * Red Hat Satellite
  * Red Hat Ansible Automation Platform

* **Managed Node**
  These are the remote systems (hosts) that you want to configure and manage. Ansible doesn't need to be installed on them.

* **Ansible Playbook**
  A YAML file defining the **desired configuration** or steps for managed nodes to execute.

* **Inventory**
  A file listing all the managed nodes, often with IP addresses, and optional grouping for better organization. Also referred to as a `hostfile`.



### **Roles Available in RHEL System Roles Package**

Each role encapsulates automation for a specific system function. On a RHEL 10 control node, you get roles for:

| Role Name            | Function                                       |
| -------------------- | ---------------------------------------------- |
| `ad_integration`     | Active Directory integration                   |
| `aide`               | Intrusion detection setup                      |
| `bootloader`         | GRUB boot loader management                    |
| `certificate`        | SSL/TLS certificate handling                   |
| `cockpit`            | Web console setup                              |
| `crypto_policies`    | Cryptographic policy configuration             |
| `fapolicy`           | File access policy settings                    |
| `firewall`           | Firewalld configuration                        |
| `ha_cluster`         | High availability cluster setup                |
| `journald`           | Journald service configuration                 |
| `kdump`              | Kernel crash dump settings                     |
| `kernel_settings`    | Kernel parameter tuning                        |
| `logging`            | Log management                                 |
| `metrics`            | Performance and metrics collection             |
| `nbde_client/server` | Network Bound Disk Encryption setup            |
| `network`            | Network interface configuration                |
| `podman`             | Container management via Podman                |
| `postfix`            | Mail service configuration                     |
| `postgresql`         | PostgreSQL database configuration              |
| `rhc`                | Red Hat Insights subscription and client setup |
| `selinux`            | SELinux policy management                      |
| `ssh/sshd`           | SSH client/server configuration                |
| `storage`            | Disk and volume management                     |
| `systemd`            | Managing services and units                    |
| `timesync`           | NTP/chrony-based time sync setup               |
| `tlog`               | Terminal session logging                       |
| `vpn`                | IPsec VPN configuration                        |



### **Further Reading and Resources**

Each role comes with detailed documentation:

* Role-specific README:
  `/usr/share/ansible/roles/rhel-system-roles.<role_name>/README.md`
* Supplementary docs:
  `/usr/share/doc/rhel-system-roles/<role_name>/`



### **Summary**

RHEL system roles provide a powerful, modular way to automate system administration tasks using Ansible, allowing administrators to maintain a secure, consistent, and scalable RHEL infrastructure.
