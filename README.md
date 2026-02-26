In System Design, **Replication** and **Sharding** are the two primary ways to scale a database. They solve different problems: Replication handles **Availability and Reads**, while Sharding handles **Storage and Writes**.

---

### 1. Database Replication

Replication is the process of keeping full copies of the same data on multiple servers.

#### **Master-Slave (Primary-Replica) Architecture**

This is the most common pattern.

* **The Master:** Handles all **Write** operations (INSERT, UPDATE, DELETE).
* **The Slaves:** Only handle **Read** operations. They sync with the master to get the latest data.
* **Pros:** Great for "Read-Heavy" apps (like YouTube or Twitter where many people view but few post).
* **Cons:** If the Master fails, the system can't take writes until a slave is promoted to Master.

#### **Sync vs. Async Replication**

* **Synchronous:** The Master waits for the Slave to confirm it received the data before telling the user "Success."
* *Pro:* Zero data loss. *Con:* Slow (limited by network speed).


* **Asynchronous:** The Master saves the data locally and tells the user "Success" immediately. It updates the Slaves in the background.
* *Pro:* Very fast. *Con:* Risk of "Replication Lag" (User reads from a slave and sees old data).



#### **Multi-Master Replication**

Multiple nodes can handle both Reads and Writes.

* **Pros:** High availability; if one master dies, others are ready.
* **Cons:** Extremely complex. You must handle "Write Conflicts" (Two people editing the same data at the same time on different masters).

---

### 2. Database Sharding

Sharding is **Horizontal Partitioning**. Instead of copying the whole database, you split the data into smaller chunks (shards) and put them on different servers.

| Feature | Replication | Sharding |
| --- | --- | --- |
| **Data** | Every server has a **full copy**. | Each server has a **different piece**. |
| **Solves** | High Read traffic & Fault tolerance. | Large Data volume & High Write traffic. |
| **Complexity** | Low to Medium. | High. |

#### **Native Support: SQL vs. NoSQL**

* **SQL (Relational):** Traditional SQL (MySQL, PostgreSQL) was built to live on one big server. It does **not** support sharding natively. You usually have to write the routing logic in your **Application Layer** or use middleware like Vitess.
* **NoSQL:** Most NoSQL databases (MongoDB, Cassandra) were built for the cloud. They have **Native Sharding** built-in. You just add a server, and the DB automatically moves data for you.

---

### 3. Sharding Strategies

How do you decide which user goes to which shard? You need a **Shard Key**.

* **Range-Based Sharding:** Split data by a range (e.g., User IDs 1-1000 go to Shard A, 1001-2000 to Shard B).
* *Pro:* Easy to implement.
* *Con:* Leads to **Hotspots**. If User IDs 1-1000 are the most active, Shard A will crash while Shard B is idle.


* **Hash-Based Sharding:** Take the `hash(User_ID) % Number_of_Shards`.
* *Pro:* Evenly distributes data.
* *Con:* If you add a new shard, the math changes for everyone, and you have to move all your data.


* **Consistent Hashing:** (The logic we discussed earlier). It maps keys to a ring.
* *Pro:* Best for scaling. Adding a shard only requires moving a small fraction of the data.



---

### Summary Checklist

* **Need more Reads?** Add Replicas.
* **Need to store more Data?** Shard it.
* **Need to scale Writes?** Shard it or use Multi-Master.

