# 📘 Huawei VRP Basics

---

## 1️⃣ What is VRP (Versatile Routing Platform)

* Huawei’s **proprietary network operating system**
* Used on **routers, switches, firewalls, ACs**
* Provides:

  * Unified **user & management interface**
  * Control plane implementation
  * Communication with forwarding plane
* Modular and component-based architecture

📌 **Exam note:**
Most Huawei datacom devices run **VRPv5**; some high-end devices use **VRPv8** .

---

## 2️⃣ VRP Versions (High Level)

* **VRPv5**

  * Widely used
  * Commands take effect **immediately**
  * No candidate configuration database
* **VRPv8**

  * Uses **candidate → commit** mechanism
  * More suitable for core devices

---

## 3️⃣ Login Methods

### Local Login

* Uses **console port**
* Enabled by default
* Used for **first-time configuration**
* Requires **console cable + terminal software (PuTTY)**
* Default baud rate: **9600**

📌 Only **ONE user** can log in via console at a time (console user ID = 0)

---

### Remote Login

* **Telnet**

  * TCP port **23**
  * Plaintext (insecure)
* **SSH**

  * TCP port **22**
  * Encrypted (recommended)
  * Disabled by default → must be configured

Remote users connect through **VTY (Virtual Terminal)** interfaces .

---

## 4️⃣ CLI Basics

* CLI = Command Line Interface
* Used to **configure, manage, and monitor** devices
* Commands follow this structure:

  * **Command word**
  * **Keyword**
  * **Parameters**

Example:

```text
display ip interface GigabitEthernet 0/0/0
```

---

## 5️⃣ Command Views (VERY IMPORTANT)

| View           | Purpose                          |
| -------------- | -------------------------------- |
| User View      | Display & diagnostic commands    |
| System View    | Global configuration             |
| Interface View | Interface-specific configuration |
| Protocol View  | Routing protocol configuration   |

📌 Navigation commands:

* `system-view`
* `quit`
* `return`

---

## 6️⃣ Online Help & Shortcuts

* `?` → show help
* `Tab` → auto-complete
* Partial keywords allowed if **unique**

Common shortcut keys:

* `Ctrl + C` → stop command
* `Ctrl + Z` → return to user view

---

## 7️⃣ File System Basics

VRP supports file and directory operations:

* `dir`
* `pwd`
* `cd`
* `mkdir`
* `rmdir`
* `delete`
* `undelete`

📌 Deleted files go to **recycle bin** first

---

## 8️⃣ Configuration Files (CRITICAL)

* Default config file: **vrpcfg.cfg**
* Configurations must be **saved manually**

Commands:

```text
save
save filename.zip
startup saved-configuration filename.zip
display startup
```

📌 Config file takes effect **after reboot**

---

## 9️⃣ Configuration Databases

| Database  | Description                      |
| --------- | -------------------------------- |
| Running   | Active configuration             |
| Startup   | Configuration used at boot       |
| Candidate | (VRPv8 only) uncommitted configs |

📌 VRPv5 → **no candidate database**
📌 VRPv8 → requires `commit` 

---

## 🔟 User Interface (VTY)

* Used for Telnet / SSH
* Usually **VTY 0–4**
* Max number varies by device
* Can configure:

  * Authentication mode
  * User privilege level
  * Allowed protocols (Telnet / SSH)
