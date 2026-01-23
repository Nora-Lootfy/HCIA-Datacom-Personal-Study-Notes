# 📘 Ethernet Switching Basics

---

## 1️⃣ Overview of Ethernet Protocols

* **Ethernet** is the **most widely used LAN protocol**
* Operates as a **broadcast-based network**
* Originally used **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection)

📌 **Modern switched Ethernet (full-duplex)** does **not use CSMA/CD**, but the concept is still important for exams.

---

### Collision Domain

* A **collision domain** is a group of devices sharing the same physical medium
* Collisions can occur **only within the same collision domain**

📌 **Each switch port = one collision domain**

---

### CSMA/CD Working Principle

1. Listen before sending
2. Transmit if the medium is idle
3. Listen while transmitting
4. Stop transmission if a collision is detected
5. Wait for a random delay, then retransmit

---

### Broadcast Domain

* A **broadcast domain** is a group of devices that receive **broadcast frames**
* By default:

  * **Switches** → do NOT break broadcast domains
  * **Routers** → break broadcast domains

---

## 2️⃣ Overview of Ethernet Frames

### Ethernet II Frame Format

| Field           | Length        |
| --------------- | ------------- |
| Destination MAC | 6 bytes       |
| Source MAC      | 6 bytes       |
| Type            | 2 bytes       |
| Data            | 46–1500 bytes |
| FCS             | 4 bytes       |

**Type Field (Common Values):**

* `0x0800` → IPv4
* `0x0806` → ARP

📌 **Ethernet II is the most commonly used frame format today**

---

### IEEE 802.3 Frame Format

| Field           | Length        |
| --------------- | ------------- |
| Destination MAC | 6 bytes       |
| Source MAC      | 6 bytes       |
| Length          | 2 bytes       |
| LLC             | 3 bytes       |
| SNAP            | 5 bytes       |
| Data            | 38–1492 bytes |
| FCS             | 4 bytes       |

---

### Logical Link Control (LLC)

LLC consists of:

* **DSAP (1 byte)** – Destination Service Access Point
* **SSAP (1 byte)** – Source Service Access Point
* **Control (1 byte)** – Usually `0x03` (connectionless service)

📌 DSAP/SSAP play a role similar to:

* Ethernet II **Type field**
* TCP/UDP **port numbers**

---

### Subnetwork Access Protocol (SNAP)

* **Org Code**: 3 bytes (always `00-00-00`)
* **Type**: 2 bytes (same function as Ethernet II Type)

---

> [!NOTE]
> Ethernet frame size (without preamble):
> **Minimum: 64 bytes**
> **Maximum: 1518 bytes**

---

## 3️⃣ MAC Address Basics

### MAC Address Definition

* A **MAC address** uniquely identifies a **Network Interface Card (NIC)**
* Length: **48 bits (6 bytes)**
* Represented in **hexadecimal**

Example:

```
00-1A-2B-3C-4D-5E
```

---

### MAC Address Composition

| Part                | Length  | Description              |
| ------------------- | ------- | ------------------------ |
| **OUI**             | 24 bits | Assigned by IEEE         |
| **Device ID (CID)** | 24 bits | Assigned by manufacturer |

---

### MAC Address Classification

| Type          | Description                     |
| ------------- | ------------------------------- |
| **Unicast**   | Least significant bit (LSB) = 0 in first byte on the left |
| **Multicast** | LSB = 1 in first byte on the left |
| **Broadcast** | FF-FF-FF-FF-FF-FF               |

📌 Broadcast frames are received by **all devices** in the broadcast domain.

---

## 4️⃣ Overview of Ethernet Switches

* Ethernet switches operate at **Layer 2 (Data Link Layer)**
* Forward frames based on **destination MAC address**
* Each switch port:

  * Is an **independent collision domain**
  * Supports **full-duplex communication**

📌 Switches **eliminate collisions** but **do not block broadcasts**

---

### MAC Address Table (CAM Table)

* Stores mappings between:

  * **MAC address**
  * **Outgoing interface**
* Built dynamically by examining **source MAC addresses**

---

## 5️⃣ Frame Processing Behaviors of a Switch

| Behavior       | Description                                                                    |
| -------------- | ------------------------------------------------------------------------------ |
| **Flooding**   | Unknown unicast or broadcast frames sent to all ports except the incoming port |
| **Forwarding** | Known unicast frames sent to the correct port                                  |
| **Discarding** | Frames dropped if destination MAC is on the incoming port                      |

---

## 6️⃣ Process of Data Communication Within a Network Segment

1. Source host sends a frame
2. Switch learns the **source MAC**
3. Switch checks destination MAC:

   * Known → forward
   * Unknown → flood
4. Destination host receives the frame
5. Switch updates its MAC table

📌 This process allows switches to **learn network topology automatically**

