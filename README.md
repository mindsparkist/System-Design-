To build scalable systems, you have to understand the "pipes" through which data flows. Here is a breakdown of the networking essentials for System Design.

---

### 1. The Models: OSI vs. TCP/IP

Think of the **OSI Model** as a theoretical map of how communication should happen, while **TCP/IP** is the actual implementation used by the internet today.

| Layer | OSI Name | Function | Protocol Example |
| --- | --- | --- | --- |
| **7** | Application | Where the user interacts | HTTP, DNS, SMTP |
| **4** | Transport | End-to-end connection | TCP, UDP |
| **3** | Network | Routing data (IP addresses) | IP, ICMP |
| **2** | Data Link | Physical addressing (MAC) | Ethernet, Wi-Fi |

---

### 2. IP & HTTP: The Core Protocols

* **IP (Internet Protocol):** Responsible for addressing and routing packets so they reach the right destination. It's like the address on an envelope.
* **HTTP (HyperText Transfer Protocol):** The language used by web browsers and servers to talk to each other. It sits on top of IP.

---

### 3. IP Address Types

* **Public IP:** Your "home address" on the global internet. It must be unique worldwide.
* **Private IP:** Your address within your local network (like your home Wi-Fi). It isn't visible to the outside world.
* **Static IP:** An address that never changes. Used for servers so clients can always find them.
* **Dynamic IP:** An address that changes periodically (assigned by your ISP via DHCP).

---

### 4. NAT, PAT, & Port Forwarding

Because there aren't enough IPv4 addresses for every device on earth, we use these tricks:

* **NAT (Network Address Translation):** Your router takes one **Public IP** and shares it with all your home devices. It translates your private request into a public one.
* **PAT (Port Address Translation):** A specific type of NAT that uses different **Port Numbers** to keep track of which internal device made which request.
* **Port Forwarding:** When you tell your router: *"If a request comes from the internet on Port 80, send it directly to my specific internal server at 192.168.1.10."*

---

### 5. Firewalls & Security

A **Firewall** is a security gatekeeper. It monitors and filters incoming/outgoing traffic based on a set of rules (e.g., "Block all traffic except on Port 443").

---

### 6. Common Ports You Must Know

In system design, "Ports" are like specific doors at an address.

| Port | Protocol | Purpose |
| --- | --- | --- |
| **22** | **SSH** | Securely logging into servers remotely. |
| **80** | **HTTP** | Standard web traffic (unencrypted). |
| **443** | **HTTPS** | Secure web traffic (encrypted). |
| **3306** | **MySQL** | Default database connection port. |
| **6379** | **Redis** | Common port for caching services. |

---

