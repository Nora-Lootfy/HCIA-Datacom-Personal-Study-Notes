# 📘 Inter-VLAN Communication

---

## 1️⃣ Background

In real-world networks, **different VLANs are usually assigned different IP subnets**.

### Layer 2 Communication

* Devices in the **same VLAN and same IP subnet**
* Can communicate **directly**
* **No Layer 3 device required**

📌 This is called **Layer 2 communication**

---

### Inter-VLAN Communication

* Communication **between different VLANs**
* Belongs to **Layer 3 communication**
* Requires a **Layer 3 device**

### Common Layer 3 Devices

* Routers
* Layer 3 switches
* Firewalls

📌 Inter-VLAN traffic must be **routed** by a Layer 3 device.

---

## 2️⃣ Inter-VLAN Communication Using Routers

### Using Physical Interfaces

* Router Layer 3 interfaces act as **default gateways**
* Router physical interfaces:

  * **Cannot process VLAN-tagged frames**
  * Must connect to **access ports** on the switch
* **One physical interface = one VLAN**

❌ **Disadvantages**

* Requires many physical interfaces
* Poor scalability
* Not suitable for large VLAN deployments

---

### Using Sub-Interfaces (Router-on-a-Stick)

A **sub-interface** is a logical Layer 3 interface created on a router’s physical interface.

#### Key Features

* Sub-interfaces **support 802.1Q VLAN tags**
* One physical interface can serve **multiple VLANs**
* Physical interface connects to a **trunk port**

📌 This method is called **Router-on-a-Stick**

---

### Traffic Processing Logic

* Switch sends **tagged frames** to the router
* Router:

  * Reads the VLAN ID
  * Forwards the packet to the corresponding sub-interface
  * Routes traffic between VLANs

---

### Configuration Example (Huawei Router)

```text
[R1] interface GigabitEthernet0/0/1.10
[R1-GigabitEthernet0/0/1.10] dot1q termination vid 10
[R1-GigabitEthernet0/0/1.10] ip address 192.168.10.254 24
[R1-GigabitEthernet0/0/1.10] arp broadcast enable
```

📌 Each VLAN requires:

* One sub-interface
* One IP address (gateway)

---

## 3️⃣ Inter-VLAN Communication Using VLANIF Interfaces

### Layer 3 Switch Concept

* A **Layer 2 switch** → switching only
* A **Layer 3 switch** → switching + routing

A **VLANIF interface** is:

* A **Layer 3 logical interface**
* Bound to a specific VLAN
* Used as the **default gateway** for that VLAN

📌 VLANIF number = VLAN ID
Example: `Vlanif 10` ↔ `VLAN 10`

---

### Configuration Example (Huawei L3 Switch)

#### VLAN & Access Port Configuration

```text
[SW1] vlan batch 10 20

[SW1] interface GigabitEthernet0/0/1
[SW1-GigabitEthernet0/0/1] port link-type access
[SW1-GigabitEthernet0/0/1] port default vlan 10

[SW1] interface GigabitEthernet0/0/2
[SW1-GigabitEthernet0/0/2] port link-type access
[SW1-GigabitEthernet0/0/2] port default vlan 20
```

#### VLANIF Configuration

```text
[SW1] interface Vlanif 10
[SW1-Vlanif10] ip address 192.168.10.254 24

[SW1] interface Vlanif 20
[SW1-Vlanif20] ip address 192.168.20.254 24
```

📌 VLANIF interfaces enable **high-performance inter-VLAN routing**

---

## 4️⃣ Layer 3 Communication Process

### Logical Connection

* Layer 3 devices forward packets based on:

  * **Destination IP address**
  * **Routing table**

---

### NAPT (Network Address Port Translation)

* Translates:

  * **Source IP address**
  * **Source port number**
* Used to allow **private IP addresses** to access **public networks**
* Multiple private IPs can share **one public IP**

📌 Commonly used for **Internet access**

---

## 5️⃣ How Are Packets Changed When Forwarded at Layer 3? ⭐ (Important)

When a packet is forwarded by a Layer 3 device:

### What Changes

* **Source MAC address** → replaced with outgoing interface MAC
* **Destination MAC address** → replaced with next-hop MAC
* **TTL value** → decremented by 1
* **IP header checksum** → recalculated
