In System Design, an **API (Application Programming Interface)** is the contract that allows two software components to communicate. If the server is a kitchen, the API is the waiter who takes your order and brings back the food.

---

### 1. API Paradigms: A Comparison

| Type | Protocol/Format | Style | Best For |
| --- | --- | --- | --- |
| **SOAP** | XML (Strict) | Action-based | Enterprise/Banking (High Security) |
| **REST** | JSON/HTTP | Resource-based | Standard Web Services |
| **GraphQL** | JSON/HTTP | Query-based | Mobile apps / Complex data |
| **gRPC** | Protobuf / HTTP/2 | Action-based | Microservices (Internal) |

---

### 2. REST (Representational State Transfer)

REST is the most common. It uses standard HTTP methods.

**Example: Get a list of users**
`GET https://api.shuvradip.com/v1/users?page=2&limit=10`

**Pagination:** You never want to fetch 1 million users at once (it would crash the app). We use pagination:

* **Offset Pagination:** `?offset=20&limit=10` (Easy, but slow for large datasets).
* **Cursor Pagination:** `?after_id=ae34-f2` (Faster and more reliable for real-time data).

---

### 3. GraphQL: The "Specific" Query Language

GraphQL is a layer that usually sits on top of **HTTP POST**. Even if you are "getting" data, you send a POST request with a "query" body.

**Why use it?**

* **No Over-fetching:** Get exactly the fields you want (e.g., just `name`, not the whole user object).
* **No Under-fetching:** Get nested data in one request (e.g., `User` + their `Posts` + `Comments`).

**The Caching Problem:** Since GraphQL uses **POST** for everything, it is **not idempotent** by default in the eyes of the browser/CDN.

* **REST:** A `GET` request is easily cached by your browser or a CDN (like Cloudflare).
* **GraphQL:** Harder to cache at the network level. You often have to use client-side caching (like Apollo Client) or "Persisted Queries."

---

### 4. gRPC: The Speed Demon

gRPC is built by Google for internal microservice communication.

* **Protocol Buffers (Protobuf):** Instead of sending bulky text (JSON), it sends **Binary**. Binary is much smaller and faster to "serialize" (convert to data).
* **HTTP/2:** It uses HTTP/2 features like **Streaming** (Server can stream data to client, or vice versa, or both).
* **The Browser Catch:** Browsers don't fully support HTTP/2 frames required by gRPC. To use it in a web app, you need a "proxy" (like gRPC-web).

---

### 5. Stateful vs. Stateless APIs

* **Stateless (REST/GraphQL):** The server doesn't remember you. You must send your "ID" (Token) with every request. This makes it **easy to scale** because any server can handle any request.
* **Stateful (WebSockets/gRPC streams):** The server keeps a connection open and "knows" who you are. Harder to scale because if the server crashes, the "state" is lost.

---

### Summary Checklist for your Repo:

* **REST:** Good for public APIs and simple CRUD.
* **GraphQL:** Good for frontend-heavy apps with complex data shapes.
* **gRPC:** Good for backend-to-backend communication where performance is king.

