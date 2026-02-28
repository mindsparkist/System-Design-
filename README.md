In System Design, **Object Storage** is the go-to solution for storing "unstructured data." Unlike a File System (hierarchy of folders) or a Block Storage (chunks of data for an OS), Object Storage treats every piece of data as a standalone unit with its own metadata and a unique identifier.

---

### 1. From BLOBs to Object Storage

The predecessor to modern Object Storage was **BLOB (Binary Large Object) Storage**.

* **BLOB Storage:** Originally, databases like MySQL or Oracle allowed you to store a "BLOB" column. However, storing massive files inside a relational database is **extremely inefficient** because it bloats the database size, slows down backups, and makes scaling difficult.
* **The Shift:** Engineers realized that "Large Objects" (images, videos) shouldn't live *inside* the database. Instead, we store the **File** in an external Object Store and store only the **URL/Metadata** in the database.

---

### 2. Key Characteristics of Object Storage

* **Flat Hierarchy:** There are no "folders" in the traditional sense. Everything sits in a flat "Bucket."
* **Metadata:** Each object can have extensive tags (e.g., `user_id: 123`, `resolution: 1080p`, `upload_date: 2026-02-28`).
* **UUID:** You access the file via a unique ID or a URL (e.g., `s3.amazonaws.com/my-bucket/shuvradip-profile.jpg`).
* **Scalability:** It is designed to scale to **Exabytes** of data across thousands of cheap servers.

---

### 3. Real-World Examples

* **AWS S3 (Simple Storage Service):** The industry standard.
* **Google Cloud Storage (GCS):** Used for massive data analytics.
* **Azure Blob Storage:** Microsoft’s implementation.
* **MinIO:** An open-source version you can run on your own private servers.

---

### 4. Primary Use Cases

Since you are documenting this, these are the "High-Value" use cases for your repo:

* **Static Assets (Images/Videos):** Every photo you see on Instagram or video on Netflix is served from Object Storage.
* **Data Lakes:** Storing massive amounts of raw logs for AI/ML training.
* **Backup & Disaster Recovery:** Because it’s cheap and durable, it's perfect for storing database backups for years.
* **Website Hosting:** You can host an entire "Static Website" (HTML/CSS/JS) directly from an S3 bucket without needing a web server like Nginx.

---

### 5. Why use it in System Design?

When designing an app like **YouTube**, you would:

1. Store the **Video File** in **Object Storage (S3)**.
2. Store the **Video Title, Views, and URL** in a **Relational DB (PostgreSQL)**.
3. Use a **CDN** to cache that video file from the Object Store closer to the user.

---

### Summary Checklist for your Repo:

* **Is it searchable?** No, you can't "grep" through Object Storage easily.
* **Is it fast for editing?** No, to change one byte, you usually have to re-upload the whole object.
* **Is it cheap?** Yes, it is the most cost-effective way to store petabytes of data.
