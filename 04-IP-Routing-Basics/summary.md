# 📘 IP Routing Basics

---

## 1️⃣ What Is IP Routing?

* **Routing** is the process of selecting a path for IP packets from **source** to **destination**
* Used to enable **communication between different IP subnets**
* Performed by **Layer 3 devices** (routers or Layer-3 switches)

📌 **Routing is path selection, forwarding is packet delivery**

---

## 2️⃣ Route-Based Forwarding

* Routers forward packets **hop-by-hop**
* Each router:

  1. Checks the **destination IP**
  2. Searches the **routing table**
  3. Selects the **best route**
  4. Forwards packet to **next hop**

📌 All routers on the path **must have routes** to the destination
📌 Communication is **bidirectional** → forward and return routes are required 

---

## 3️⃣ Routing Table (Key Fields)

| Field            | Description                     |
| ---------------- | ------------------------------- |
| Destination/Mask | Target network                  |
| Protocol         | How the route was learned       |
| Preference       | Route priority (lower = better) |
| Cost             | Metric (lower = better)         |
| Next Hop         | IP address of next router       |
| Interface        | Outbound interface              |

---

## 4️⃣ Route Selection Rules (VERY IMPORTANT)

Routers select routes in the following order:

1. **Longest prefix match**
2. **Lowest preference value**
3. **Lowest cost (metric)**
4. **Equal-cost routes → ECMP**

📌 **Lower preference value = higher priority** 

---

## 5️⃣ Route Preference Examples

| Route Type | Default Preference |
| ---------- | ------------------ |
| Direct     | 0                  |
| OSPF       | 10                 |
| IS-IS      | 15                 |
| Static     | 60                 |
| RIP        | 100                |

📌 Dynamic routes usually override static routes unless modified

---

## 6️⃣ Static Routing

### Characteristics

* Manually configured
* Simple and stable
* No automatic adaptation to topology changes

### Configuration Commands

```text
[Huawei] ip route-static <destination> <mask> <next-hop>
```


Example:

```text
[Huawei] ip route-static 20.1.1.0 255.255.255.0 10.0.0.2
```

---

### Interface-Based Static Route

```text
[Huawei] ip route-static <destination> <mask> <interface>
```

📌 **Point-to-point interface → must specify interface**
📌 **Broadcast interface → must specify next hop** 

---

## 7️⃣ Default Route (Special Static Route)

* Used when **no specific route matches**
* Destination: **0.0.0.0/0**

### Configuration

```text
[Huawei] ip route-static 0.0.0.0 0.0.0.0 <next-hop>
```

📌 Commonly used for Internet access 

---

## 8️⃣ Floating Route (Backup Route)

* A **static route with higher preference**
* Used only when the primary route fails

### Example

```text
[Huawei] ip route-static 10.0.0.0 24 192.168.1.1 preference 100
```

📌 Larger preference value = **lower priority** 

---

## 9️⃣ Equal-Cost Multi-Path (ECMP)

* Multiple routes to same destination
* Same:

  * Prefix
  * Preference
  * Cost
* Router load-balances traffic

📌 Improves bandwidth usage and redundancy

---

## 🔟 Route Recursion

* Occurs when the **next hop is not directly connected**
* Router recursively looks up a route to the next hop
* Stops when a **direct route** is found

📌 Excessive recursion can impact performance 

---

## 1️⃣1️⃣ Route Summarization (Aggregation)

* Combines multiple routes into **one summary route**
* Uses **CIDR**
* Reduces routing table size

### Example

Specific routes:

```text
10.1.1.0/24
10.1.2.0/24
10.1.3.0/24
```

Summary route:

```text
10.1.0.0/20
```

### Configuration

```text
[Huawei] ip route-static 10.1.0.0 20 12.1.1.2
```

📌 Summary routes require **common prefix bits** 

---

## 1️⃣2️⃣ Static vs Dynamic Routing

| Feature       | Static        | Dynamic          |
| ------------- | ------------- | ---------------- |
| Configuration | Manual        | Automatic        |
| Scalability   | Low           | High             |
| Adaptation    | No            | Yes              |
| Complexity    | Simple        | Complex          |
| Examples      | Default route | OSPF, RIP, IS-IS |

📌 Dynamic routing is preferred in **large networks** 

