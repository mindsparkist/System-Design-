DNS is the "Phonebook of the Internet." It translates human-readable names like `google.com` into machine-readable IP addresses like `142.250.190.46`.

---

### 1. The Structure of a URL

A URL (Uniform Resource Locator) is broken down into specific parts that tell the browser where to go and what to do:
`https://www.example.com:443/blog/article?id=10#comments`

* **Scheme/Protocol:** `https://` (How to connect).
* **Subdomain:** `www` (A specific section of the domain).
* **Domain Name:** `example` (The name registered with a registrar).
* **Top-Level Domain (TLD):** `.com` (The suffix: .org, .net, .edu).
* **Port:** `:443` (The specific "door" on the server).
* **Path:** `/blog/article` (The specific resource on the server).
* **Query Parameters:** `?id=10` (Extra data sent to the server).
* **Fragment/Anchor:** `#comments` (A specific spot on the page).

---

### 2. DNS Hierarchy & ICANN

* **ICANN:** The global non-profit that manages the entire DNS namespace.
* **Registrars:** Companies (like GoDaddy or Namecheap) where you "rent" a domain. They report back to the Registry managed by ICANN.
* **ISP (Internet Service Provider):** Usually provides your first point of contact for DNS queries.

---

### 3. Recursive vs. Iterative Queries

This is the "Search Process."

* **Recursive Query:** The client (your computer) asks the **Recursive Resolver** (usually your ISP) to "find the answer and don't come back until you have it."
* **Iterative Query:** The Resolver then goes out and asks other servers. If a server doesn't know, it says, "I don't know, but try asking this guy."
1. **Root Nameservers:** "I don't know 'example.com', but I know who handles **.com**."
2. **TLD Nameservers:** "I know who handles **example.com**."
3. **Authoritative Nameservers:** "I am the boss of 'example.com', here is the IP!"



---

### 4. DNS Records & A Records

The DNS database uses different types of "records":

* **A Record:** Maps a domain to an **IPv4** address.
* **AAAA Record:** Maps a domain to an **IPv6** address.
* **CNAME:** An alias (maps one domain to another domain, e.g., `blog.com` -> `mainserver.com`).
* **MX Record:** Directs email to the correct mail server.

---

### 5. DNS Caching

To avoid doing the whole "Root-to-Authoritative" trip every time, DNS results are saved (cached) at multiple levels:

* **Browser Cache:** Your browser remembers sites you just visited.
* **OS Cache:** Windows/Mac keeps a local temporary file.
* **ISP Cache:** Your provider caches popular sites for all its users.
* **TTL (Time to Live):** A setting on the DNS record that tells the cache how long to keep the data before asking for an update.

---

### 6. Tools and Advanced Concepts

* **nslookup:** A command-line tool used to query DNS.
* *Try it:* Open your terminal and type `nslookup google.com`. It will show you the IP address and which server provided the answer.


* **Dynamic DNS (DDNS):** Useful for home servers where the ISP changes your IP address frequently. A DDNS service automatically updates your **A Record** whenever your IP changes.

---

