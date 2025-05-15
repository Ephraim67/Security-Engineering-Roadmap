# Disabling Windows Services that could be a Security Risk

## A Windows services are insecure if:
- unpatched services can introduce vulnerability
- Be exploited by malware or attackers
- Consume system ressources unnecessarily
- Service binary path is modifiable by Everyone
- The service is running as SYSTEM or a privileged account
- Service permissions (SDDL) allow non-admins to modify configs

# Windows Services and Right Access



## **Access Rights for the Service Control Manager (SCM)**

The **Service Control Manager** is the component of Windows responsible for managing all installed services. Access rights control what a user or process can do with it.



### **Specific Access Rights**

| Access Right                    | Hex       | Description                                                                                                                    |
| ------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `SC_MANAGER_ALL_ACCESS`         | `0xF003F` | Full control over the SCM. Includes all rights listed below plus standard rights. Only **Administrators** typically have this. |
| `SC_MANAGER_CONNECT`            | `0x0001`  | Connects to the SCM database. This is the **minimum right** needed to perform any further action.                              |
| `SC_MANAGER_CREATE_SERVICE`     | `0x0002`  | Allows creation of a **new service** via `CreateService()` function. This is a **privileged action**.                          |
| `SC_MANAGER_ENUMERATE_SERVICE`  | `0x0004`  | Allows listing of all services via `EnumServicesStatus()` or `EnumServicesStatusEx()`.                                         |
| `SC_MANAGER_LOCK`               | `0x0008`  | Allows **locking the service database**, used to prevent modifications during edits.                                           |
| `SC_MANAGER_QUERY_LOCK_STATUS`  | `0x0010`  | Allows checking if the database is locked.                                                                                     |
| `SC_MANAGER_MODIFY_BOOT_CONFIG` | `0x0020`  | Allows changing boot-time configuration (rarely used).                                                                         |



### **Generic Access Rights**

These are **shortcuts** that combine several specific rights.

| Generic Right     | Includes                                                                               |
| ----------------- | -------------------------------------------------------------------------------------- |
| `GENERIC_READ`    | `STANDARD_RIGHTS_READ`, `SC_MANAGER_ENUMERATE_SERVICE`, `SC_MANAGER_QUERY_LOCK_STATUS` |
| `GENERIC_WRITE`   | `STANDARD_RIGHTS_WRITE`, `SC_MANAGER_CREATE_SERVICE`, `SC_MANAGER_MODIFY_BOOT_CONFIG`  |
| `GENERIC_EXECUTE` | `STANDARD_RIGHTS_EXECUTE`, `SC_MANAGER_CONNECT`, `SC_MANAGER_LOCK`                     |
| `GENERIC_ALL`     | `SC_MANAGER_ALL_ACCESS`                                                                |



###  **Use Case: Penetration Testing**

When exploiting services:

* If a low-privileged user has **`SC_MANAGER_CREATE_SERVICE`**, they can register a malicious service to escalate privileges.
* If the user has **`SC_MANAGER_CONNECT` + `ENUMERATE_SERVICE`**, they can enumerate and inspect services for weak configurations.
* If `EVERYONE` or `Authenticated Users` are given **write permissions** to the SCM or a service, it becomes exploitable (e.g., via `sc sdshow`).



###  **Who Gets What Access**

| Account Type                                                       | Rights Granted                                                                                               |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| Remote Authenticated Users                                         | `SC_MANAGER_CONNECT`                                                                                         |
| Local Authenticated Users (incl. `LocalService`, `NetworkService`) | `SC_MANAGER_CONNECT`, `SC_MANAGER_ENUMERATE_SERVICE`, `SC_MANAGER_QUERY_LOCK_STATUS`, `STANDARD_RIGHTS_READ` |
| `LocalSystem`                                                      | All above + `SC_MANAGER_MODIFY_BOOT_CONFIG`                                                                  |
| Administrators                                                     | `SC_MANAGER_ALL_ACCESS`                                                                                      |

>  **Note**: A user logged in over the network (RDP or SMB) may not have the same rights as someone logged in interactively (direct login or local console). Full SCM control typically requires **interactive admin privileges**.



### **Security Descriptor of SCM**

* SCM itself has a **security descriptor** that defines which users can perform which actions.

* You can query it with:

  ```powershell
  sc sdshow SCMANAGER
  ```

* You can modify it with:

  ```powershell
  sc sdset SCMANAGER D:(A;;CCLCSWRPWPDTLOCRRC;;;BA)
  ```

  > This would, for example, give full access to the **Built-in Administrators (BA)** group.



## TL;DR for Exploitation

| Objective                                 | Right Needed                    |
| ----------------------------------------- | ------------------------------- |
| List all services                         | `SC_MANAGER_ENUMERATE_SERVICE`  |
| Connect to SCM                            | `SC_MANAGER_CONNECT`            |
| Install new service (e.g., reverse shell) | `SC_MANAGER_CREATE_SERVICE`     |
| Lock service database                     | `SC_MANAGER_LOCK`               |
| Modify startup behavior                   | `SC_MANAGER_MODIFY_BOOT_CONFIG` |

