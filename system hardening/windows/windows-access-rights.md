##  **Specific Access Rights for a Service**

| **Access Right**               | **Hex Value** | **Description**                                                                                                                    |
| ------------------------------ | ------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `SERVICE_ALL_ACCESS`           | `0xF01FF`     | Full control over the service (includes all other service rights and standard rights). Only administrators should be granted this. |
| `SERVICE_CHANGE_CONFIG`        | `0x0002`      | Allows changing the service settings (e.g., executable path, startup type). Critical for service configuration.                    |
| `SERVICE_ENUMERATE_DEPENDENTS` | `0x0008`      | Lets you list services that depend on this service. Useful for understanding service dependencies.                                 |
| `SERVICE_INTERROGATE`          | `0x0080`      | Lets you query the service's current state (e.g., running, paused). Typically used for monitoring.                                 |
| `SERVICE_PAUSE_CONTINUE`       | `0x0040`      | Allows pausing or resuming the service. Important for services that support these operations.                                      |
| `SERVICE_QUERY_CONFIG`         | `0x0001`      | Allows reading the service’s configuration.                                                                                        |
| `SERVICE_QUERY_STATUS`         | `0x0004`      | Lets you check the current status (running, stopped, etc.).                                                                        |
| `SERVICE_START`                | `0x0010`      | Allows starting the service.                                                                                                       |
| `SERVICE_STOP`                 | `0x0020`      | Allows stopping the service.                                                                                                       |
| `SERVICE_USER_DEFINED_CONTROL` | `0x0100`      | Lets you send custom control codes to a service (for custom behaviors).                                                            |


##  **Standard Access Rights for a Service**

These are rights common across many Windows objects.

| **Access Right**         | **Hex Value** | **Description**                                                                                          |
| ------------------------ | ------------- | -------------------------------------------------------------------------------------------------------- |
| `ACCESS_SYSTEM_SECURITY` | —             | Lets you access or modify the **System Access Control List (SACL)**. Requires the `SeSecurityPrivilege`. |
| `DELETE`                 | `0x10000`     | Allows deletion of the service.                                                                          |
| `READ_CONTROL`           | `0x20000`     | Lets you read the service’s security descriptor (permissions).                                           |
| `WRITE_DAC`              | `0x40000`     | Allows you to change the DACL (who has access to the service).                                           |
| `WRITE_OWNER`            | `0x80000`     | Allows changing the owner of the service object.                                                         |



##  **Generic Access Rights for a Service**

These combine specific and standard rights for convenience:

| **Access Right**  | **Maps to...**                                                                                                        |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- |
| `GENERIC_READ`    | `READ_CONTROL`, `SERVICE_QUERY_CONFIG`, `SERVICE_QUERY_STATUS`, `SERVICE_INTERROGATE`, `SERVICE_ENUMERATE_DEPENDENTS` |
| `GENERIC_WRITE`   | `WRITE_DAC`, `SERVICE_CHANGE_CONFIG`                                                                                  |
| `GENERIC_EXECUTE` | `SERVICE_START`, `SERVICE_STOP`, `SERVICE_PAUSE_CONTINUE`, `SERVICE_USER_DEFINED_CONTROL`                             |
| `GENERIC_ALL`     | `SERVICE_ALL_ACCESS`                                                                                                  |



## **Default Access Based on Account Type**

| **Account**                                                            | **Default Rights**                                                                                                                                    |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Remote authenticated users**                                         | Typically none. *(On Windows Server 2003 SP1 and later, they may get `SERVICE_USER_DEFINED_CONTROL`)*                                                 |
| **Local authenticated users** (e.g., `LocalService`, `NetworkService`) | `READ_CONTROL`, `SERVICE_QUERY_CONFIG`, `SERVICE_QUERY_STATUS`, `SERVICE_INTERROGATE`, `SERVICE_ENUMERATE_DEPENDENTS`, `SERVICE_USER_DEFINED_CONTROL` |
| **LocalSystem**                                                        | Everything above, plus: `SERVICE_START`, `SERVICE_STOP`, `SERVICE_PAUSE_CONTINUE`                                                                     |
| **Administrators**                                                     | `SERVICE_ALL_ACCESS`, `DELETE`, `WRITE_DAC`, `WRITE_OWNER`, full control                                                                              |


## **Security Implications**

* **Be careful** granting rights like:

  * `SERVICE_CHANGE_CONFIG`: lets someone change the executable the service runs—**can lead to privilege escalation**.
  * `SERVICE_STOP`: can disrupt critical services.
* **Only trusted users** (typically admins) should have these.
* When services are listed using `EnumServicesStatusEx`, services for which the caller lacks `SERVICE_QUERY_STATUS` are **omitted silently** from the result.


## **Working with Access Rights in Code**

* Use `OpenService()` to open a handle to a service with specific access rights.
* Use `QueryServiceObjectSecurity()` or `SetServiceObjectSecurity()` to **view or modify** the service’s security descriptor.
* Rights are enforced via ACLs (Access Control Lists), and only granted if the process’s access token permits it.
