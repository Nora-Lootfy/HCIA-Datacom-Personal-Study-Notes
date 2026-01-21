# 📘 Network Layer Protocols & IP Addressing

---

## 1️⃣ Network Layer Overview

* The **network layer** is also called the **IP layer**
* Provides **connectionless** data transmission
* Each packet is sent **independently** (no session establishment)

**Main functions:**

* Logical addressing
* Packet routing and forwarding
* Path selection

📌 **PDU name:** Packet 

---

## 2️⃣ Network Layer Protocols

Common protocols at this layer:

* **IP (IPv4 / IPv6)** – core protocol
* **ICMP** – error reporting & diagnostics
* **IGMP** – multicast group management

📌 IP is responsible for **addressing and forwarding**, not reliability.  
---

## 3️⃣ IPv4 Basics

* IPv4 address length: **32 bits**
* Written in **dotted-decimal format**
* Consists of:

  * **Network part (Network ID)**
  * **Host part (Host ID)**

📌 Only devices in the **same network ID** can communicate directly.

---

## 4️⃣ IPv4 Address Classification (Classful)

| Class | First Bits | Address Range               | Network Bits | Host Bits |
| ----- | ---------- | --------------------------- | ------------ | --------- |
| A     | 0          | 0.0.0.0 – 127.255.255.255   | 8            | 24        |
| B     | 10         | 128.0.0.0 – 191.255.255.255 | 16           | 16        |
| C     | 110        | 192.0.0.0 – 223.255.255.255 | 24           | 8         |
| D     | 1110       | 224.0.0.0 – 239.255.255.255 | Multicast    | —         |
| E     | 1111       | 240.0.0.0 – 255.255.255.255 | Experimental | —         |

📌 **Only Class A, B, C** are used for host addressing .

---

## 5️⃣ Special IPv4 Addresses

* **Network address** → all host bits = 0
* **Broadcast address** → all host bits = 1
* **127.0.0.0/8** → loopback
* **0.0.0.0** → unspecified address
* **169.254.0.0/16** → APIPA
* **Private IP ranges:**

  * 10.0.0.0/8
  * 172.16.0.0/12
  * 192.168.0.0/16

---

## 6️⃣ Subnetting & VLSM

* Subnetting divides a large network into **smaller subnets**
* Uses **Variable Length Subnet Mask (VLSM)**
* Benefits:

  * Reduces IP address waste
  * Improves network management
  * Supports hierarchical design

📌 Subnetting **increases routing entries**, which leads to the need for route summarization.

---

## 7️⃣ Route Summarization (Aggregation)

* Combines multiple routes into **one summary route**
* Reduces routing table size
* Uses **CIDR**

Example:

```text
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
↓
192.168.0.0/22
```

📌 Summary routes must share **common prefix bits** .

---

## 8️⃣ IPv4 Packet Header

**IPv4 Header**  
<p align="center">
  <img src="images/IPv4Header.png" width="600">
</p>

<br>

| Field Name                 | Length   | Description                                                                                               |
| -------------------------- | -------- | --------------------------------------------------------------------------------------------------------- |
| **Version**                | 4 bits   | Identifies the IP version. <br>• `4` → IPv4 <br>• `6` → IPv6                                              |
| **Header Length (IHL)**    | 4 bits   | Indicates the size of the IP header. <br>• Minimum: **20 bytes** (no options) <br>• Maximum: **60 bytes** |
| **Type of Service (ToS)**  | 8 bits   | Specifies packet service type. <br>Used when **QoS / DiffServ** is required                               |
| **Total Length**           | 16 bits  | Total length of the IP packet (header + data)                                                             |
| **Identification**         | 16 bits  | Identifies fragments for **reassembly**                                                                   |
| **Flags**                  | 3 bits   | Controls fragmentation (DF, MF)                                                                           |
| **Fragment Offset**        | 13 bits  | Indicates the position of a fragment during reassembly                                                    |
| **Time To Live (TTL)**     | 8 bits   | Limits packet lifetime. <br>Decrements by 1 at each router                                                |
| **Protocol**               | 8 bits   | Identifies the next-layer protocol                                                                        |
| **Header Checksum**        | 16 bits  | Error-checking for the IP header                                                                          |
| **Source IP Address**      | 32 bits  | IP address of the sender                                                                                  |
| **Destination IP Address** | 32 bits  | IP address of the receiver                                                                                |
| **Options**                | Variable | Optional fields (rarely used)                                                                             |
| **Padding**                | Variable | Filled with `0`s to align header to 32 bits                                                               |


| Value | Protocol | Description                        |
| ----- | -------- | ---------------------------------- |
| 1     | ICMP     | Internet Control Message Protocol  |
| 2     | IGMP     | Internet Group Management Protocol |
| 6     | TCP      | Transmission Control Protocol      |
| 17    | UDP      | User Datagram Protocol             |


* **TTL (Time To Live)**

  * Decreases by 1 at each router
  * Prevents routing loops

* **Source & Destination IP**

📌 If TTL reaches **0**, the packet is dropped and ICMP is sent back .

---

## 9️⃣ ICMP (Internet Control Message Protocol)

* Used for:

  * Error reporting
  * Network diagnostics
* Common messages:

  * Destination Unreachable
  * Time Exceeded
  * Echo Request / Reply (ping)

📌 ICMP is **not** used to transmit application data.

**ICMP Message format**  

| Field                    | Length   | Description                                    |
| ------------------------ | -------- | ---------------------------------------------- |
| **Type**                 | 8 bits   | Identifies the ICMP message type               |
| **Code**                 | 8 bits   | Provides additional information about the type |
| **Checksum**             | 16 bits  | Error checking for the ICMP message            |
| **ICMP Message Content** | Variable | Depends on the ICMP type and code              |


**ICMP Type and code**  

| Type  | Code | Description          |
| ----- | ---- | -------------------- |
| **0** | 0    | Echo Reply           |
| **3** | 0    | Network Unreachable  |
| **3** | 1    | Host Unreachable     |
| **3** | 2    | Protocol Unreachable |
| **3** | 3    | Port Unreachable     |
| **5** | 0    | Redirect             |
| **8** | 0    | Echo Request         |


---

## 🔟 Gateway Concept

* A **gateway** forwards packets between **different networks**
* Default gateway = router interface IP in the local network
* Hosts send all **non-local traffic** to the gateway

📌 Without a default gateway, a host cannot reach other networks .

---

## 1️⃣1️⃣ IPv4 vs IPv6 (Quick Compare)

| Feature            | IPv4           | IPv6           |
| ------------------ | -------------- | -------------- |
| Address length     | 32 bits        | 128 bits       |
| Address exhaustion | Yes            | No             |
| NAT required       | Yes            | No             |
| Auto-configuration | DHCP           | SLAAC / DHCPv6 |
| Security           | Optional IPsec | Built-in IPsec |

