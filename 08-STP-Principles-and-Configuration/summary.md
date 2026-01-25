# 📘 STP Principles and Configuration

---

## 1️⃣ STP Overview

**Spanning Tree Protocol (STP)** is used in **LANs** to:

* Detect **Layer 2 loops**
* Block **redundant links**
* Convert a looped topology into a **loop-free tree**

This prevents:

* Broadcast storms
* MAC address table instability
* Infinite frame looping

STP runs continuously and:

* Monitors network topology
* Detects topology changes
* Automatically recalculates paths to maintain network reliability

📌 With the growth of LANs, STP is a **core Layer 2 protocol**.

---

## 2️⃣ Basic STP Concepts

### Bridge ID (BID)

* Defined in **IEEE 802.1D**
* Uniquely identifies a switch in an STP network

**BID Structure:**

* **Bridge Priority** (16 bits)
* **MAC Address** (48 bits)

📌 The switch with the **smallest BID** becomes the **Root Bridge**.

---

### Root Bridge (RB)

* Central reference point of the spanning tree
* All path calculations are made **toward the root bridge**

---

### Path Cost & Root Path Cost (RPC)

* Each STP-enabled port has a **path cost**
* **Root Path Cost (RPC)** = total cost from the switch to the root bridge
* Used to select the **best path** to the root

---

### Port ID (PID)

* Identifies switch ports during elections

**PID Structure:**

* **Port Priority** (4 bits)
* **Port Number** (12 bits)

📌 Used when multiple ports have equal path cost.

---

### BPDU (Bridge Protocol Data Unit)

* STP control packets exchanged between switches
* Carry:

  * Root bridge information
  * Path cost
  * Timers
* Used to calculate and maintain the spanning tree

---

## 3️⃣ STP Operation Process

STP performs the following steps:

1. **Elect the Root Bridge**
2. Each non-root switch selects a **Root Port**

   * Lowest RPC
   * Lowest upstream BID
   * Lowest upstream PID
3. Each network segment selects a **Designated Port**

   * Lowest RPC
   * Lowest BID
   * Lowest PID
4. All other ports are **blocked**

📌 Only **Root Ports** and **Designated Ports** forward traffic.

---

## 4️⃣ STP Port States

| State          | Description                             |
| -------------- | --------------------------------------- |
| **Disabled**   | Port is down; no BPDUs or data          |
| **Blocking**   | Receives BPDUs only; no data forwarding |
| **Listening**  | Participates in STP calculation         |
| **Learning**   | Learns MAC addresses, no forwarding     |
| **Forwarding** | Forwards data and processes BPDUs       |

📌 Only **Forwarding** ports carry user traffic.

---

## 5️⃣ STP Failure and Recovery Scenarios

### 🔹 Root Bridge Failure

* Non-root switches stop receiving BPDUs
* **Max Age timer (20s)** expires
* New root bridge election starts
* Ports transition:

  * Blocking → Listening → Learning → Forwarding

⏱ **Convergence time ≈ 50 seconds**
(Max Age + 2 × Forward Delay)

---

### 🔹 Direct Link Failure

* Root port fails
* Alternate port becomes new root port
* Port enters Forwarding after **30 seconds**
  (2 × Forward Delay)

---

### 🔹 Unphysical Link Failure

* Link is logically down (no BPDU received)
* Max Age expires
* Temporary incorrect root election may occur
* Network re-converges after BPDU comparison

⏱ **Convergence time ≈ 50 seconds**

---

## 6️⃣ MAC Address Table Issues After Topology Changes

* MAC table aging time = **300 seconds**
* After topology changes:

  * MAC entries may point to wrong ports
  * Frames may be forwarded incorrectly

📌 STP triggers **MAC table updates** to avoid forwarding errors.

---

## 7️⃣ Basic STP Configuration (Huawei)

### Configure STP Mode

```text
[Huawei] stp mode { stp | rstp | mstp }
```

---

### Enable STP

```text
[Huawei] stp enable
```

---

### Configure Root Bridge

```text
[Huawei] stp root primary
[Huawei] stp root secondary
```

---

### Configure Bridge Priority

```text
[Huawei] stp priority priority
```

---

### Configure Path Cost Calculation Method

```text
[Huawei] stp pathcost-standard { dot1d-1998 | dot1t | legacy }
```

📌 All switches must use the **same cost calculation method**.

---

### Configure Port Cost

```text
[Huawei-GigabitEthernet0/0/1] stp cost cost
```

---

### Display STP Status

```text
[Huawei] display stp brief
```

---

## 8️⃣ Rapid Spanning Tree Protocol (RSTP – IEEE 802.1w)

### RSTP Improvements

* Faster convergence
* Backward compatible with STP
* Introduces:

  * **Alternate ports**
  * **Backup ports**
  * **Edge ports**

---

### RSTP Port Roles

| Role                | Description                |
| ------------------- | -------------------------- |
| **Root Port**       | Best path to root          |
| **Designated Port** | Best port on a segment     |
| **Alternate Port**  | Backup for root port       |
| **Backup Port**     | Backup for designated port |

---

### Edge Port

* Connects to end devices
* Enters **Forwarding state immediately**
* No BPDU exchange

📌 Similar to **PortFast** in Cisco

---

### STP vs RSTP Port States

| STP State  | RSTP State | Port Role          |
| ---------- | ---------- | ------------------ |
| Forwarding | Forwarding | Root / Designated  |
| Learning   | Learning   | Root / Designated  |
| Listening  | Discarding | Root / Designated  |
| Blocking   | Discarding | Alternate / Backup |
| Disabled   | Discarding | Disabled           |

---

## 9️⃣ STP Advancements

### 🔹 VLAN-Based Spanning Tree (VBST)

* Huawei proprietary
* One spanning tree **per VLAN**
* Enables **VLAN load balancing**

---

### 🔹 Multiple Spanning Tree Protocol (MSTP – IEEE 802.1s)

* Solves STP/RSTP limitations
* Groups VLANs into **MST Instances (MSTIs)**
* Benefits:

  * Reduced resource usage
  * Load balancing across VLANs
  * Faster convergence

📌 VLANs mapped to same MSTI share topology.

---

## 🔟 Campus Network Enhancements

### Intelligent Stack (iStack)

* Multiple switches act as **one logical device**
* Single management IP & MAC
* Improves:

  * Reliability
  * Performance
  * Simplified management

---

### Smart Link

* Designed for **dual-uplink access**
* One link active, one standby
* No protocol packets exchanged
* Millisecond-level failover

📌 Eliminates loops without STP.

