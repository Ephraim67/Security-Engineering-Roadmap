##  **1. Traditional Policy (Deprecated / Optional)**

* Uses names like `eth0`, `eth1`, etc.
* Interface names are assigned **based on detection order** at boot.
* **Unreliable**: Interface names can change if hardware is added/removed or if devices initialize in a different order.
* Can be re-enabled using the kernel boot options:

  ```bash
  net.ifnames=0 biosdevname=0
  ```



##  **2. Predictable Network Interface Names (Default Policy in RHEL)**

Enabled by default starting with **RHEL 7**. This policy uses **hardware location and firmware data** to assign consistent names.

### The naming follows specific schemes:

| Scheme     | Example           | Explanation                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| `enoN`     | `eno1`            | Onboard device number N                          |
| `ensN`     | `ens3`            | Hotplug slot index N                             |
| `enpXsY`   | `enp0s3`          | PCI bus X, slot Y                                |
| `enx<MAC>` | `enx001122334455` | MAC address-based (used when other schemes fail) |



##  **3. BIOS-Provided Names (biosdevname)**

* Optional, but used in some enterprise server environments.
* Names interfaces based on **BIOS labels**, such as `em1`, `p1p1`, etc.
* Enabled with:

  ```bash
  biosdevname=1
  ```

  (Note: `biosdevname` must be installed and supported by your hardware)


##  **4. Custom `udev` Rules**

* You can create your own naming rules using the **MAC address** or **device path**.
* Example:

  ```bash
  /etc/udev/rules.d/70-persistent-net.rules
  ```
* Example rule:

  ```
  SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="00:11:22:33:44:55", NAME="custom0"
  ```



## Recap of Policy Order

RHEL prefers naming policies in this order:

1. **Onboard index** (`eno`)
2. **Hotplug slot index** (`ens`)
3. **PCI bus location** (`enpXsY`)
4. **MAC address** (`enx<MAC>`)
5. **Traditional (`eth0`)** only if predictable naming is explicitly disabled
