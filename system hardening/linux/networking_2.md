## **How `udev` Renames Network Interfaces**

### **1. Kernel Detects Devices**

* When the system boots, the **Linux kernel** detects all hardware devices, including network interfaces.
* Each device has **unique attributes** like:

  * PCI bus location
  * MAC address
  * Firmware-provided device paths

---

### **2. `udev` Collects Device Info**

* The **`udev` device manager** listens to these detection events.
* It gathers detailed **hardware metadata** (e.g., `/sys/class/net/*`).
* This metadata includes:

  * PCI path
  * Slot number
  * Onboard index
  * MAC address
  * Device type



### **3. `udev` Applies Naming Rules**

* `udev` applies a **built-in naming policy** (defined by `systemd`) based on the metadata.
* The policy chooses **stable and unique names** using rules in order of preference:

  1. **Firmware/device tree info** (e.g., onboard devices → `eno1`)
  2. **PCI hotplug slot info** (e.g., `ens1`)
  3. **PCI path info** (e.g., `enp0s3`)
  4. **MAC address** (used only if all others fail)



### **4. Interface Gets Renamed**

* Based on the chosen rule, `udev` **renames the interface** from its original (e.g., `eth0`) to the consistent name (e.g., `enp0s3`).
* This name appears in `ip a`, `ifconfig`, `nmcli`, etc.



### **5. Interface Naming is Persistent**

* Because the name is based on **physical topology**, it remains stable:

  * **Across reboots**
  * **If devices are added or removed**
* No need for manual renaming via config files.



## **Example**

If your Ethernet interface is connected to PCI bus 0, slot 3:

* `udev` will name it:
  **`enp0s3`**
  (`en` = Ethernet, `p0` = bus 0, `s3` = slot 3)



## **Overriding udev Naming (Optional)**

If you want to **override** the default naming:

* You can create a custom `udev` rule in:

  ```bash
  /etc/udev/rules.d/70-persistent-net.rules
  ```
* Or pass a kernel boot option:

  ```bash
  net.ifnames=0 biosdevname=0
  ```

  This reverts naming back to `eth0`, `eth1`, etc.

