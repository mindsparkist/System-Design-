Caching is one of the most powerful tools in your system design arsenal. It’s essentially the "short-term memory" of your application, designed to make data retrieval lightning-fast.

---

### 1. The Speed Hierarchy: Cache > RAM > Disk

In computing, there is always a trade-off between **size** and **speed**.

| Storage Layer | Type | Speed (Latencey) | Capacity |
| --- | --- | --- | --- |
| **L1/L2/L3 Cache** | SRAM (on CPU) | < 1 nanosecond | KBs to MBs |
| **RAM (Memory)** | DRAM (off CPU) | ~10-100 nanoseconds | GBs |
| **SSD/HDD (Disk)** | Flash/Magnetic | 10 microseconds - 10ms | TBs |

> **The Lesson:** Accessing data from Disk is like driving across the country to get a book; RAM is like walking to your bookshelf; and Cache is like having the book open in your hands.

---

### 2. Cache Hit, Miss, and Ratio

* **Cache Hit:** The system finds the requested data in the cache. (Result: Fast).
* **Cache Miss:** The data isn't in the cache; the system must go to the "Slow Disk/Database" to find it. (Result: Slow).
* **Cache Hit Ratio:** The percentage of requests served by the cache.

$$Cache\ Hit\ Ratio = \frac{Hits}{Hits + Misses} \times 100\%$$



*A "good" ratio depends on the app, but typically >80-90% is excellent.*

---

### 3. Server-Side Write Strategies

When you update data, you have to decide how the **Cache** and the **Database (DB)** stay in sync.

* **Write-Through:** Data is written to the **Cache AND the DB** at the same time.
* *Pros:* Strong consistency. If the system crashes, the DB is up to date.
* *Cons:* Higher latency (you wait for two writes).


* **Write-Around:** Data is written **directly to the DB**, bypassing the cache.
* *Pros:* Doesn't "pollute" the cache with data that might not be read soon.
* *Cons:* A "Cache Miss" will occur the first time that data is read.


* **Write-Back (Write-Behind):** Data is written **only to the Cache** first. The DB is updated later (asynchronously).
* *Pros:* Incredible speed (fastest write performance).
* *Cons:* Risk of data loss if the cache crashes before it syncs to the DB.



---

### 4. HTTP Cache Headers (The Browser's Perspective)

How a server tells a browser to cache:

* **Cache-Control:** The most important header. `max-age=3600` means cache for 1 hour.
* **ETag:** A "fingerprint" of the file. If the file hasn't changed, the server sends a **304 Not Modified**, saving bandwidth.
* **Expires:** An older header using a specific date (e.g., `Wed, 21 Oct 2025 07:28:00 GMT`).

---

### 5. Eviction Policies: Who gets kicked out?

When the cache is full, someone has to go.

* **FIFO (First In, First Out):** The oldest item added is the first to leave. Simple, but "dumb"—it might kick out a very popular item just because it's old.
* **LRU (Least Recently Used):** Discards the item that hasn't been looked at for the longest time. **(The Industry Favorite)**.
* **LFU (Least Frequently Used):** Discards the item with the lowest "hit count." Good for keeping "viral" content cached.

---

### 6. Good vs. Bad Ideas for Caching

* **Good:**
* **Static Content:** Images, CSS, JS.
* **Slow Queries:** Complex DB joins that don't change often.
* **User Sessions:** Frequent lookups of "Is this user logged in?".


* **Bad:**
* **Highly Sensitive Data:** Don't cache passwords or bank balances in a shared cache.
* **Rapidly Changing Data:** If it changes every millisecond, the overhead of updating the cache is more expensive than just reading the DB.



[This System Design Fundamentals video](https://www.youtube.com/watch?v=BaKtC-VIYrM) provides a high-level overview of how caching and other components fit into the big picture of a scalable architecture.
