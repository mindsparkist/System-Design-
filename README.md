You’ve got the core logic down perfectly. In System Design, the choice between TCP and UDP usually comes down to whether you care more about **data integrity** or **speed**.

---

### 1. TCP (Transmission Control Protocol)

As you noted, TCP is "connection-oriented." It ensures that every single packet arrives in the correct order and without errors.

* **The 3-Way Handshake:** Before any data moves, the client and server agree to talk:
1. **SYN** (Synchronize)
2. **SYN-ACK** (Acknowledge)
3. **ACK** (Connected!)


* **Key Features:** Flow control (prevents overwhelming the receiver) and congestion control (slows down if the network is busy).
* **Pros:** Guaranteed delivery, data arrives in order, easy to use for developers.
* **Cons:** Higher latency (due to handshakes and acknowledgments), larger header size (20 bytes).

---

### 2. UDP (User Datagram Protocol)

UDP is "connectionless." It just starts sending packets. If one gets lost, it doesn't care; it just moves on to the next one.

* **"Fire and Forget":** There is no handshake and no acknowledgment that the data was received.
* **Pros:** Extremely fast, low overhead (8-byte header), supports broadcasting (sending to many people at once).
* **Cons:** No guarantee of delivery, packets can arrive out of order, no built-in congestion control.

---

### 3. Comparison Table

| Feature | TCP | UDP |
| --- | --- | --- |
| **Reliability** | Guaranteed | Best Effort (No guarantee) |
| **Ordering** | Sequenced | Not ordered |
| **Speed** | Slower (Overhead) | Faster (Streamlined) |
| **Retransmission** | Retransmits lost data | Never retransmits |
| **Header Size** | 20 Bytes | 8 Bytes |

---

### 4. Real-World Use Cases

In a System Design interview, you’ll be asked to choose one based on the app you are building:

* **Use TCP for:**
* **Web Browsing (HTTP/HTTPS):** You can't have a website load with half the text missing.
* **File Transfers (FTP):** A corrupted file is useless.
* **Database Connections:** Data must be accurate and consistent.


* **Use UDP for:**
* **Video Streaming/VoIP:** If a single frame of a Zoom call is lost, you'd rather skip it than freeze the whole video to wait for it.
* **Online Gaming:** Speed and "real-time" feel are more important than 100% accuracy.
* **DNS:** Lookups need to be lightning-fast.



---

### Advanced Note: QUIC (HTTP/3)

Interestingly, the industry is moving toward **QUIC**, which is a protocol built **on top of UDP** but adds its own reliability layer. It gives you the speed of UDP with the safety of TCP. This is what modern browsers and apps like YouTube use to stay fast!
