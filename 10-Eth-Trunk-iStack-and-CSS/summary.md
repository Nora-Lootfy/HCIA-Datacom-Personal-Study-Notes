# 📘 Ether Trunk, iStack, and CSS

---

## 1️⃣ Network Reliability Requirements

**Network reliability** refers to the ability of a network to provide **continuous services** even when:

* A single component fails
* Multiple components fail

Reliability can be implemented at **three levels**:

* Card level
* Device level
* Link level

---

## 2️⃣ Card Reliability

A **modular switch** consists of the following components:

| Component        | Function                                              |
| ---------------- | ----------------------------------------------------- |
| **Chassis**      | Provides slots and interconnection between modules    |
| **Power Module** | Supplies power to the device                          |
| **Fan Module**   | Provides heat dissipation                             |
| **MPU**          | Handles control plane and management plane            |
| **SFU**          | Handles the data plane and high-speed switching       |
| **LPU**          | Provides physical interfaces and forwarding functions |

📌 Redundant power supplies, MPUs, and fans improve card-level reliability.

---

## 3️⃣ Device Reliability

### ❌ No Redundancy

* Downstream switch has **only one uplink**
* Failure of upstream switch or link causes **network interruption**

---

### ✅ Master / Backup Mode

* Downstream switch is **dual-homed** to two upstream switches
* Links operate in **active/backup mode**
* If the active link or device fails, traffic switches to the backup path

📌 Improves availability and fault tolerance.

---

## 4️⃣ Link Reliability

* Deploy **multiple physical links** between devices
* To prevent loops:

  * Use **STP**
  * Only one link forwards traffic
  * Other links act as backups

📌 This ensures redundancy but does **not increase bandwidth**.

---

## 5️⃣ Ether Trunk (Link Aggregation)

### Why Ether Trunk?

* STP blocks redundant links → bandwidth wasted
* **Ether Trunk (Eth-Trunk)** bundles multiple physical links into **one logical link**
* Increases bandwidth **without upgrading hardware**

---

### Basic Eth-Trunk Concepts

| Term                    | Description                                    |
| ----------------------- | ---------------------------------------------- |
| **LAG**                 | Logical link formed by multiple physical links |
| **Eth-Trunk Interface** | Logical interface of the LAG                   |
| **Member Interface**    | Physical interface in the Eth-Trunk            |
| **Active Link**         | Participates in data forwarding                |
| **Inactive Link**       | Backup link                                    |
| **Aggregation Mode**    | Manual or LACP                                 |

---

## 6️⃣ Link Aggregation Modes

### 🔹 Manual Mode

* Member interfaces configured manually
* No negotiation between devices
* All links usually forward traffic
* Suitable when **peer device does not support LACP**

❌ **Limitations**

* No protocol verification
* Peer configuration must be manually ensured
* Can only detect failures at the **physical layer**

---

### 🔹 LACP Mode (IEEE 802.3ad)

* Uses **LACPDU** packets for negotiation
* Ensures:

  * Peer interfaces belong to the same Eth-Trunk
  * Peer interfaces are on the same device

**LACPDU contains:**

* System priority
* Device MAC address
* Interface priority
* Interface number

📌 LACP improves reliability and prevents misconfiguration.

---

## 7️⃣ LACP Key Parameters

### System Priority

* Determines the **Actor**
* Lower value = higher priority
* Actor decides which links are active

---

### Interface Priority

* Determines which interfaces are selected
* Lower value = higher priority

---

### Maximum Active Interfaces

* Limits the number of active links
* Extra links become **backup links**
* If an active link fails, the highest-priority inactive link replaces it

📌 Ensures **stable bandwidth and fast recovery**.

---

## 8️⃣ Load Balancing in Eth-Trunk

### ❌ Per-Packet Load Balancing

* Packets of the same flow may arrive **out of order**
* Causes performance issues

---

### ✅ Per-Flow Load Balancing (Recommended)

* Frames of the same flow use the **same physical link**
* Ensures packet order
* Achieves effective load balancing

---

### Load Balancing Criteria

Traffic can be distributed based on:

* Source IP
* Destination IP
* Source MAC
* Destination MAC
* Source & destination IP
* Source & destination MAC

📌 Choose fields that **change frequently** for better distribution.

---

## 9️⃣ Typical Application Scenarios

### Between Switches

* Multiple links aggregated into Eth-Trunk
* Improves bandwidth and reliability

---

### Between Switch and Server

* Server NIC bonding
* Eth-Trunk improves access bandwidth and redundancy

---

### Between Switch and iStack

* Eth-Trunk connects a switch to a stacked device
* Forms a reliable, loop-free topology

---

### Firewall Heartbeat Link

* Eth-Trunk used as heartbeat link
* Prevents false failover caused by single-link failure

---

## 🔧 Eth-Trunk Configuration (Huawei)

```text
[Huawei] interface eth-trunk trunk-id
[Huawei-Eth-Trunk1] mode { lacp | manual load-balance }
```

Add member interfaces:

```text
[Huawei-GigabitEthernet0/0/1] eth-trunk trunk-id
```

Or from Eth-Trunk view:

```text
[Huawei-Eth-Trunk1] trunkport interface-type interface-number
```

Enable mixed-rate interfaces:

```text
[Huawei-Eth-Trunk1] mixed-rate link enable
```

Configure LACP:

```text
[Huawei] lacp priority priority
[Huawei-GigabitEthernet0/0/1] lacp priority priority
```

Configure active links:

```text
[Huawei-Eth-Trunk1] max active-linknumber number
[Huawei-Eth-Trunk1] least active-linknumber number
```

---

## 🔟 iStack and CSS Overview

### iStack

* Multiple fixed switches form **one logical switch**
* Managed using **one IP address**
* Used to:

  * Increase port density
  * Improve reliability

---

### CSS (Cluster Switch System)

* Two modular switches form **one logical device**
* Unified:

  * Control plane
  * Forwarding plane
* Inter-device links aggregated as Eth-Trunk

📌 CSS removes the need for:

* MSTP
* VRRP

---

## 🔑 Key Advantages of iStack & CSS

* Simplified network management
* Faster convergence
* High availability
* Load balancing across devices
* Improved scalability

