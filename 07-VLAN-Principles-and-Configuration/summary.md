# 📘 VLAN Principles and Configuration

---

## 1️⃣ What Is a VLAN?

A **VLAN (Virtual Local Area Network)** logically divides a physical network into **multiple broadcast domains**.

### Key Characteristics

* VLANs **isolate broadcast traffic**
* Devices in different VLANs **cannot communicate directly** without a Layer 3 device
* VLANs improve:

  * Network security
  * Performance
  * Manageability

📌 **Each VLAN = one broadcast domain**

---

## 2️⃣ VLAN Principles

### IEEE 802.1Q VLAN Tagging

* IEEE **802.1Q** defines a **4-byte VLAN tag**
* The tag is inserted into the Ethernet frame

### 802.1Q Frame Format

| D.MAC | S.MAC | VLAN Tag | Length / Type | Data | FCS |
| ----- | ----- | -------- | ------------- | ---- | --- |

**VLAN Tag (4 bytes) includes:**

* **TPID (16 bits)** – Tag Protocol Identifier

  * Value `0x8100` indicates an 802.1Q-tagged frame
* **PRI (3 bits)** – Frame priority (QoS)
* **CFI (1 bit)** – Canonical Format Indicator

  * Value = `0` for Ethernet
* **VLAN ID (12 bits)** – Identifies VLAN (1–4094)

> [!NOTE]
> Frames are identified as VLAN-tagged when the **Type/Length field = 0x8100**

---

## 3️⃣ VLAN Assignment Methods

### Common VLAN Assignment Techniques

| Method              | Description                                      |
| ------------------- | ------------------------------------------------ |
| **Interface-based** | VLAN assigned per switch port (PVID)             |
| **MAC-based**       | VLAN assigned based on source MAC address        |
| **IP subnet-based** | VLAN assigned based on source IP address         |
| **Protocol-based**  | VLAN assigned based on protocol type             |
| **Policy-based**    | VLAN assigned based on defined matching policies |

📌 **Interface-based VLAN assignment is the most common**

---

## 4️⃣ Layer 2 Ethernet Interface Types

### Access Interface

* Connects to **end devices** (PCs, servers)
* Sends and receives **untagged frames**
* Belongs to **only one VLAN**

---

### Trunk Interface

* Connects **switch-to-switch** or switch-to-router
* Carries **multiple VLANs**
* Uses **802.1Q tagging**

---

### Hybrid Interface

* Similar to trunk
* Can send:

  * Tagged frames for some VLANs
  * Untagged frames for others
* Flexible VLAN forwarding

📌 Hybrid interfaces are commonly used in **Huawei networks**

---

## 5️⃣ VLAN Applications

### Common VLAN Design Methods

* **By service**

  * Voice VLAN
  * Video VLAN
  * Data VLAN
* **By department**

  * Engineering
  * Marketing
  * Finance
* **By application**

  * Servers
  * Offices
  * Classrooms

---

## 6️⃣ VLAN Configuration (Huawei)

---

### Create VLANs

```text
[Huawei] vlan vlan-id
```

Create multiple VLANs:

```text
[Huawei] vlan batch vlan-id1 [ to vlan-id2 ]
```

---

### Access Interface Configuration

```text
[Huawei-GigabitEthernet0/0/1] port link-type access
[Huawei-GigabitEthernet0/0/1] port default vlan vlan-id
```

---

### Trunk Interface Configuration

```text
[Huawei-GigabitEthernet0/0/1] port link-type trunk
[Huawei-GigabitEthernet0/0/1] port trunk allow-pass vlan { vlan-id1 [ to vlan-id2 ] | all }
```

(Optional) Configure trunk PVID:

```text
[Huawei-GigabitEthernet0/0/1] port trunk pvid vlan vlan-id
```

---

### Hybrid Interface Configuration

```text
[Huawei-GigabitEthernet0/0/1] port link-type hybrid
```

Allow untagged VLANs:

```text
[Huawei-GigabitEthernet0/0/1] port hybrid untagged vlan { vlan-id1 [ to vlan-id2 ] | all }
```

Allow tagged VLANs:

```text
[Huawei-GigabitEthernet0/0/1] port hybrid tagged vlan { vlan-id1 [ to vlan-id2 ] | all }
```

(Optional) Configure hybrid PVID:

```text
[Huawei-GigabitEthernet0/0/1] port hybrid pvid vlan vlan-id
```

---

## 7️⃣ MAC Address–Based VLAN Configuration

Associate a MAC address with a VLAN:

```text
[Huawei-vlan10] mac-vlan mac-address mac-address [ mac-address-mask | mac-address-mask-length ]
```

Enable MAC-based VLAN on an interface:

```text
[Huawei-GigabitEthernet0/0/1] mac-vlan enable
```
