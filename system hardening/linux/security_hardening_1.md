### **Step 1: Understand What FIPS Is**

* **FIPS (Federal Information Processing Standards)** 140 is a standard created by NIST.
* It ensures **cryptographic modules** (e.g., encryption libraries) are **secure and validated**.
* **FIPS 140-3** is the latest version, with requirements like **self-checks and integrity tests**.

---

### **Step 2: FIPS Mode Must Be Enabled During Installation**

* **You *cannot* switch to FIPS mode after installing RHEL 10.**

  * The `fips-mode-setup` tool used in earlier RHEL versions has been **removed**.
* **Important:** You must enable FIPS **during the OS installation**.

---

### **Step 3: Start Installation in FIPS Mode**

* Boot the RHEL 10 installer with the `fips=1` option in the **kernel command line**.

  * If `/boot` is on a separate partition, add:
    `boot=UUID=<uuid-of-boot-disk>`
* The installer will then:

  * Enable FIPS
  * Configure cryptographic modules and disk encryption correctly

---

### **Step 4: Why Enabling FIPS During Install Matters**

* FIPS-compliant key material **must be generated in FIPS mode**.
* Changing to FIPS later would:

  * Require re-generating all cryptographic keys.
  * Risk **non-compliance** or **system failure** (e.g., LUKS disk encryption using Argon2 instead of PBKDF2).

---

### **Step 5: How to Verify If FIPS Mode Is Enabled**

* Check this file:
  `/proc/sys/crypto/fips_enabled`

  * `1` means FIPS mode is active
  * `0` means FIPS mode is not active

---

### **Step 6: Understand System-Wide Crypto Policies**

* RHEL has a **system-wide cryptographic policy** layer.
* When FIPS is enabled (`fips=1`), the crypto policies will:

  * **Exclude non-FIPS-approved algorithms** (e.g., ChaCha20)
  * Enforce **FIPS-only ciphers** for applications using system crypto libraries

---

### **Step 7: Ignore Application-Level FIPS Settings**

* If RHEL is already in FIPS mode, **app-level FIPS toggles are ignored**.

  * E.g., `--enable-fips` for Node.js has **no effect** if the OS is in FIPS mode.
* If you use app-level FIPS settings **without** enabling FIPS OS-wide, **you are not compliant**.

---

### **Step 8: Understand OpenSSL FIPS Indicators in RHEL 10**

* RHEL 10.0 has **its own implementation** of OpenSSL FIPS indicators.
* These may differ from upstream OpenSSL in the future.
* Expect changes or errors like `"unsupported"` if the API shifts.

---

### **Step 9: To Disable FIPS Mode**

* You **cannot disable** FIPS mode after installation.
* The only way to turn it off is to **reinstall RHEL without enabling FIPS**.

