HTTP is the language of the web. It is the protocol that allows your browser to request a page and the server to send it back.

---

### 1. The Client-Server Model

HTTP operates on a simple **Request-Response** model.

* **Client:** The "requester" (usually your browser or a mobile app).
* **Server:** The "provider" (a machine waiting to serve data).
The client opens a connection, sends a request, the server processes it, sends a response, and (historically) the connection closes.

### 2. Is HTTP Stateless?

**Yes.** By design, HTTP is **stateless**. This means the server does not "remember" the previous request.

* If you log in on Request A, Request B doesn't automatically know who you are.
* **The Fix:** We use **Cookies** or **Tokens (JWT)** to "attach" state to these stateless requests.
* **Connection Note:** While HTTP is stateless, it almost always runs on top of **TCP**, which is stateful (it maintains the connection).

### 3. HTTP vs. RPC (Remote Procedure Call)

In System Design, you’ll choose between these two for service-to-service communication:

* **HTTP (REST):** Focuses on **Resources**. You use URLs like `/users/10`. It's very flexible and works everywhere.
* **RPC (e.g., gRPC):** Focuses on **Actions**. It feels like calling a function locally in your code, e.g., `getUser(10)`. It is usually much faster because it uses binary data instead of bulky text.

---

### 4. HTTP Methods & Status Codes

**Common Methods:**

* **GET:** Retrieve data (Safe/Idempotent).
* **POST:** Create new data.
* **PUT:** Update/Replace existing data.
* **DELETE:** Remove data.

**Status Codes (The "First Digit" Rule):**

* **1xx:** Informational (Hold on...)
* **2xx:** Success (Got it! **200 OK**, **201 Created**)
* **3xx:** Redirection (Go over there... **301 Moved Permanently**)
* **4xx:** Client Error (You messed up... **404 Not Found**)
* **5xx:** Server Error (I messed up... **500 Internal Server Error**)

---

### 5. SSL/TLS & Public Key Cryptography

HTTPS is just HTTP inside a "secure pipe" created by **TLS** (the successor to SSL).

**Public Key Cryptography (Asymmetric Encryption):**
This is how the "secure pipe" is built without needing to share a secret password beforehand.

1. **Public Key:** Like an open padlock. Anyone can use it to lock (encrypt) a message.
2. **Private Key:** Like the only key that fits the lock. Only the server has this to unlock (decrypt) the message.

---

### 6. The "neverssl.com" Trick

Websites like **neverssl.com** are actually very useful for developers and IT professionals.

* **The Solution:** By visiting a site that **never** uses SSL, the Wi-Fi can successfully intercept the request and redirect you to the login page without a security error.

**Now that we've covered the "how" of communication, would you like to discuss Load Balancers—the tools that sit in front of these servers to manage all this HTTP traffic?**
