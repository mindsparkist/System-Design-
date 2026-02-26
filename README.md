The **CAP Theorem** is the fundamental trade-off of distributed systems. It was proposed by Eric Brewer and essentially states that in a distributed data store, you can only provide two out of three guarantees at any given time.

---

### 1. The Three Pillars of CAP

1. **C - Consistency:** Every read receives the most recent write or an error. (All nodes see the same data at the same time).
2. **A - Availability:** Every request receives a (non-error) response, without the guarantee that it contains the most recent write. (The system stays up even if some nodes are down).
3. **P - Partition Tolerance:** The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

---

### 2. Is Partition Tolerance Optional?

**No.** In a modern distributed system (like one running across multiple servers at Deloitte or on AWS), **Network Partitions (P) will happen.** Cables get cut, routers fail, or latency spikes.

* **For a Single Node:** CAP doesn't really apply. If you only have one server, there is no "network partition" between nodes, so you have Consistency and Availability by default—until the server crashes.
* **For Replicated Data:** Since P is a fact of life, you are forced to choose between **Consistency (CP)** and **Availability (AP)** when a partition occurs.

---

### 3. The Trade-off during a Partition

Imagine a network split where Node A cannot talk to Node B.

* **CP (Consistency over Availability):** If a user tries to write to Node A, the system says "I can't talk to Node B to sync this, so I will return an Error." The system is consistent but **unavailable**.
* *Examples:* **HBase, MongoDB, Redis** (usually).


* **AP (Availability over Consistency):** If a user writes to Node A, the system says "Got it!" and doesn't wait to tell Node B. The system is available, but Node B now has **stale data**.
* *Examples:* **Cassandra, CouchDB, DynamoDB**.



---

### 4. Moving Beyond: The PACELC Theorem

The CAP theorem only tells us what happens when there is a **failure (Partition)**. But what about when things are running normally? **PACELC** extends CAP to cover the "normal" state.

**PAC (Partition $\rightarrow$ Availability vs. Consistency)**

* **IF** there is a **P**artition:
* Do you choose **A**vailability or **C**onsistency?



**ELC (Else $\rightarrow$ Latency vs. Consistency)**

* **E**lse (when the network is running fine):
* Do you choose **L**atency (speed) or **C**onsistency?



#### **Why PACELC is better:**

Some databases are designed to be fast during normal times but consistent during failures, or vice versa.

* **DynamoDB/Cassandra (PA/EL):** If a partition happens, they choose **A**vailability. Else, they choose **L**atency (speed) over consistency.
* **MongoDB (PC/EC):** It usually aims for **C**onsistency in both states, making it a "Strictly Consistent" system.

---

### Summary for your Repository:

* **CAP** is for **Failure** scenarios.
* **PACELC** is for **Both** normal and failure scenarios.
* **Consistency** is expensive (it takes time/latency).
* **Availability** is "risky" (it might serve old data).

**Would you like to try a real-world scenario now? For example, "If you are designing the 'Like' count for a viral post, would you choose an AP or a CP system?"**
