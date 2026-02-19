In System Design, "good" isn't about being perfect; it's about making the right **trade-offs**. A good design meets the current requirements, handles expected growth (scalability), and doesn't break the bank.

Here is the breakdown of the core metrics and concepts you asked about:

---

### 1. Availability vs. Reliability

While they sound similar, they measure different things:

* **Availability:** The percentage of time the system is operational. It focuses on "uptime."
* **Reliability:** The probability that a system will perform its function without failure over a specific period.
* *Analogy:* An umbrella that opens 100% of the time is **available**. If it develops a hole after 5 minutes of rain, it is **unreliable**.



### 2. The "Service Level" Trio (SLI, SLO, SLA)

Think of these as a pyramid, moving from technical metrics to legal contracts.

* **SLI (Indicator):** What you measure (e.g., Latency, Error Rate).
* **SLO (Objective):** The target goal for the SLI (e.g., "99.9% of requests must be faster than 200ms").
* **SLA (Agreement):** The legal contract with the user. "If we don't meet our SLO, we pay you back."

### 3. Calculating Availability (The "Nines")

Availability is calculated based on the total downtime in a year.


| Availability % | Downtime per Year | Common Term |
| --- | --- | --- |
| **99%** | 3.65 days | "Two Nines" |
| **99.9%** | 8.77 hours | "Three Nines" |
| **99.99%** | 52.6 minutes | "Four Nines" |
| **99.999%** | 5.26 minutes | "Five Nines" (Gold Standard) |

---

### 4. Handling Failure: Fault Tolerance & Redundancy

* **Fault Tolerance:** The ability of a system to continue operating properly even if some components fail.
* **Redundancy:** The method used to achieve fault tolerance. It involves duplicating critical components.
* **Active-Passive:** One server works; the other waits for a crash.
* **Active-Active:** Both servers work simultaneously to share the load.



---

### 5. Throughput vs. Latency

These are the two main ways we measure speed:

* **Latency:** The time it takes for a **single** request to travel from point A to point B (measured in milliseconds).
* **Throughput:** The **volume** of requests a system can handle in a given time (e.g., 5,000 requests per second).

> **Important Note:** You can have high latency and high throughput. Imagine a massive cargo ship: it takes a long time to arrive (high latency), but it carries a massive amount of goods (high throughput).

---

### 6. Scalability: Vertical vs. Horizontal

* **Vertical Scaling (Scaling Up):** Adding more power (CPU, RAM) to your existing machine. It's easy but has a hard ceiling.
* **Horizontal Scaling (Scaling Out):** Adding more machines to your pool. This is the foundation of modern cloud systems.

