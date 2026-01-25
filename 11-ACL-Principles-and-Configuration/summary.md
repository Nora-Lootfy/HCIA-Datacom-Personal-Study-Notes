# 📘 ACL - Access Control List

---

## 1️⃣ What Is an ACL?

An **ACL (Access Control List)** is a set of **rules** used to:

* **Match packets** based on conditions
* **Permit or deny traffic**

ACLs are widely used for:

* Traffic filtering
* Security control
* QoS classification
* NAT traffic selection
* Routing policy control

📌 **ACL itself does NOT block traffic**
👉 It must be **applied to a service or interface**.

---

## 2️⃣ How ACL Works (Core Logic)

1. Packet arrives at a device
2. ACL rules are checked **top to bottom**
3. First matching rule is applied
4. If no rule matches → **implicit deny**

📌 **Rule order matters**

---

## 3️⃣ ACL Components

Each ACL rule consists of:

* **Rule ID**
* **Action**: `permit` or `deny`
* **Matching conditions**:

  * Source IP
  * Destination IP
  * Protocol
  * Port number (for advanced ACLs)

---

## 4️⃣ ACL Types in Huawei

### 🔹 Basic ACL (2000–2999)

* Matches **source IP address only**

```text
[Huawei] acl 2000
[Huawei-acl-basic-2000] rule permit source 192.168.1.0 0.0.0.255
```

📌 Used for **simple filtering**

---

### 🔹 Advanced ACL (3000–3999)

* Matches:

  * Source IP
  * Destination IP
  * Protocol
  * Port number

```text
[Huawei] acl 3000
[Huawei-acl-adv-3000] rule permit tcp source 192.168.1.0 0.0.0.255 destination any eq 80
```

📌 Used for **precise traffic control**

---

### 🔹 Layer 2 ACL (4000–4999)

* Matches **MAC addresses**
* Used in **Layer 2 environments**

```text
[Huawei] acl 4000
[Huawei-acl-layer2-4000] rule permit source-mac 00e0-fc12-3456
```

---

## 5️⃣ Wildcard Mask (Very Important)

Huawei ACLs use **wildcard masks**, not subnet masks.

| Value | Meaning            |
| ----- | ------------------ |
| `0`   | Must match exactly |
| `1`   | Ignore this bit    |

Example:

```text
192.168.1.0 0.0.0.255
```

→ Matches **192.168.1.0 – 192.168.1.255**

📌 **Wildcard = inverse of subnet mask**

---

## 6️⃣ Rule Numbering & Priority

* Rules are processed by **rule ID**
* Smaller rule number = **higher priority**

```text
rule 5 deny source 10.0.0.0 0.255.255.255
rule 10 permit source any
```

📌 Always place **deny rules before permit rules**

---

## 7️⃣ Applying an ACL (Key Concept)

ACL must be **referenced by a service** to take effect.

### Apply ACL to Interface (Traffic Filtering)

```text
[Huawei-GigabitEthernet0/0/1] traffic-filter inbound acl 3000
```

* `inbound` → filters incoming traffic
* `outbound` → filters outgoing traffic

---

## 8️⃣ Common ACL Application Scenarios

| Scenario                          | ACL Type         |
| --------------------------------- | ---------------- |
| Block specific hosts              | Basic ACL        |
| Control service access (HTTP/FTP) | Advanced ACL     |
| Restrict MAC access               | Layer 2 ACL      |
| NAT traffic selection             | Basic / Advanced |
| QoS classification                | Advanced ACL     |

---

## 9️⃣ ACL Default Behavior (EXAM TRAP)

* ACL has **implicit deny all**
* If no rule matches → packet is **discarded**

📌 Always add a **permit any** rule if needed.

---

## 🔟 Display & Management Commands

```text
display acl all
display acl 2000
display this
```

Delete ACL:

```text
undo acl 2000
```

Delete rule:

```text
undo rule 10
```
