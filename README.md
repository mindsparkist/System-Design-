A **CDN (Content Delivery Network)** is a globally distributed network of "Edge Servers" that work together to provide fast delivery of internet content.

In a traditional setup, if your server is in New York and a user in Hyderabad tries to access it, the data has to travel across the world, causing high **latency**. A CDN solves this by storing a copy of your content (images, JS, CSS, videos) in data centers (Points of Presence or PoPs) located near the user.

---

### 1. How it Works: The Origin & The Edge

* **Origin Server:** The source of truth. This is your actual server (e.g., an AWS EC2 or S3 bucket) where the original files live.
* **Edge Server:** The "cache" servers located in different cities. They sit at the "edge" of the network, as close to the user as possible.

---

### 2. Push CDN vs. Pull CDN

In System Design, you must choose how the content gets from your **Origin** to the **Edge**.

#### **Pull CDN (The "Lazy" Approach)**

This is the most common model. The CDN "pulls" the content only when it's requested.

* **How it works:**
1. A user requests `image.jpg` from the Edge server.
2. If the Edge doesn't have it (**Cache Miss**), it "pulls" it from the Origin.
3. The Edge stores it for the next user and serves it.


* **Pros:** Very low maintenance; automatically handles updates based on TTL (Time to Live).
* **Cons:** The very first user to request a file experiences high latency because the CDN has to go back to the Origin.
* **Best for:** Small files, frequently updated content, and websites with unpredictable traffic.

#### **Push CDN (The "Proactive" Approach)**

You take responsibility for "pushing" the content to the CDN servers before anyone asks for it.

* **How it works:**
1. Whenever you upload a new file to your server, your system automatically "pushes" it to the CDN network.
2. The content is already sitting at the Edge when the first user arrives.


* **Pros:** Zero latency for the first user. You have full control over what is cached and for how long.
* **Cons:** Higher maintenance. You must manually push updates or clear the cache (invalidate) whenever a file changes. It can also waste storage if you push files that no one ever requests.
* **Best for:** Large files (videos, software installers) and predictable content (like a new movie release on Netflix).

---

### 3. Comparison Summary

| Feature | Pull CDN | Push CDN |
| --- | --- | --- |
| **Effort** | Low (Automatic) | High (Manual/Scripted) |
| **First Request** | Slower (Cache Miss) | Fastest (Pre-cached) |
| **Storage Use** | Efficient (Only popular items) | Potentially wasteful |
| **Updating Content** | Wait for TTL to expire | You must push the update |
