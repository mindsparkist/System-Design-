In a distributed system, you often need to decide which server should store which piece of data. **Consistent Hashing** is the industry-standard solution for doing this in a way that allows you to add or remove servers without causing a "storm of re-shuffling."

---

### 1. What is Consistent Hashing?

In traditional hashing, we use the formula: $server\_index = hash(key) \pmod n$ (where $n$ is the number of servers).

* **The Problem:** If you add one more server ($n+1$), the result of the modulo operation changes for almost every key. You would have to move nearly **100%** of your data to new servers.
* **The Solution:** Consistent Hashing maps both **Servers** and **Data Keys** onto a virtual "Hash Ring" (a circle with a huge range, e.g., $0$ to $2^{32}-1$).

**How it works:**

1. **Place Servers:** Hash each server's ID/IP and place it on the ring.
2. **Place Data:** Hash the data key and place it on the same ring.
3. **Assign:** To find which server holds a key, go **clockwise** from the key's position until you hit the first server. That is the owner.

---

### 2. Virtual Nodes (VNodes)

If you only have 3 physical servers, they might be placed unevenly on the ring, causing one server to handle 70% of the data.

* **The Fix:** We use **Virtual Nodes**. Each physical server is hashed multiple times (e.g., Server A-1, Server A-2, Server A-3) and placed at different spots on the ring. This ensures a much more uniform distribution of data.

---

### 3. Consistent Hashing vs. Rendezvous Hashing

Both solve the "minimal data movement" problem, but they use different logic.

| Feature | Consistent Hashing | Rendezvous Hashing (HRW) |
| --- | --- | --- |
| **Logic** | "The Circle": Find the next server clockwise. | "The Highest Score": $weight = hash(key + server)$. Pick the max weight. |
| **Lookup Time** | $O(\log N)$ (using binary search on the ring). | $O(N)$ (must calculate score for every server). |
| **State** | Needs to maintain a "Ring" structure in memory. | Stateless; just needs the list of active servers. |
| **Load Balance** | Needs virtual nodes for evenness. | Naturally very even without extra work. |

---

### 4. Real-World Use Cases

* **Content Delivery Networks (CDNs):** Used by Akamai to map URLs to specific edge servers. If one edge server goes down, only the files on *that* server are moved to the next one.
* **Distributed Databases:** Used by **Amazon DynamoDB** and **Apache Cassandra** to partition data across clusters.
* **Distributed Caching:** Used by Memcached to decide which node should store a specific cache key.

---

### 5. When to use (and when not to)

* **✅ Use it when:**
* You have a **dynamic** cluster where nodes are frequently added or removed (auto-scaling).
* You are building a **stateful** service where you want the same key to consistently land on the same server.
* You have a massive number of nodes (because $O(\log N)$ lookup is faster than $O(N)$ for huge $N$).


* **❌ Don't use it when:**
* Your number of servers is **fixed** and will never change (standard modulo hashing is simpler and faster).
* You have a very small number of servers (Rendezvous hashing might be easier to implement and more balanced).
