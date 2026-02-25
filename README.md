SQL (Structured Query Language) is the backbone of **Relational Databases (RDBMS)**. Unlike NoSQL, SQL databases are built on the principles of strict structure and mathematical relationships.

---

### 1. The Physical Layer: B+ Trees

Most SQL databases (like MySQL's InnoDB or PostgreSQL) use a **B+ Tree** as their primary data structure for indexing.

* **Why B+ Trees?** They are designed to stay balanced, ensuring that searching for any record takes $O(\log N)$ time.
* **The "Plus":** In a B+ Tree, all data is stored at the "leaf" nodes, and these leaves are linked together. This makes **Range Queries** (e.g., "Find all users aged 20 to 30") incredibly fast because the database can just find the first leaf and follow the links to the next one.

---

### 2. The Logical Layer: Schema, Keys, and Joins

* **Schema:** This is the "blueprint." You must define your columns and data types (int, string, date) *before* you insert data.
* **Primary Key (PK):** A unique identifier for a row (e.g., `user_id`). It cannot be null.
* **Foreign Key (FK):** A column that creates a link between two tables. It points to a PK in another table.
* **Joins:** This is where the "Relational" part shines. You can combine data from multiple tables (Inner, Left, Right, Full) based on their related keys.

---

### 3. ACID Compliance: The Gold Standard

Relational databases are designed to be **ACID compliant** to ensure data integrity, especially during crashes or simultaneous updates.

| Property | Meaning | Simple Rule |
| --- | --- | --- |
| **Atomicity** | The "All or Nothing" rule. | If any part of a transaction fails, the whole thing is rolled back. |
| **Consistency** | Data must follow all rules. | You can't have a negative bank balance if a rule forbids it. |
| **Isolation** | Transactions don't interfere. | Two people buying the last ticket at the same time won't crash the system. |
| **Durability** | Once committed, it's permanent. | Even if the power goes out right after a "Commit," the data is safe on disk. |

---

### 4. Isolation & Locks

To achieve **Isolation**, databases use **Locks**:

* **Shared Lock (S):** Multiple transactions can *read* a row, but no one can change it.
* **Exclusive Lock (X):** Only one transaction can *write* or *read* that row. No one else is allowed in.

---

### 5. Read Phenomena (The "Glitches")

If your isolation level isn't strict enough, you run into these three classic problems:

1. **Dirty Read:** Transaction A reads data that Transaction B changed but **hasn't committed yet**. If B rolls back, A is left with "fake" data.
2. **Non-Repeatable Read:** Transaction A reads a row. Transaction B **updates** that row and commits. Transaction A reads it again and sees different values.
3. **Phantom Read:** Transaction A reads a range of rows (e.g., all users from India). Transaction B **inserts** a new user from India. Transaction A reads again and sees a "phantom" row that wasn't there before.

---

### 6. Isolation Levels

SQL databases let you choose how much you want to trade "speed" for "safety":

* **Read Uncommitted:** Fastest, but allows everything (Dirty, Non-repeatable, Phantom).
* **Read Committed:** Prevents Dirty Reads. (Default for many).
* **Repeatable Read:** Prevents Dirty and Non-repeatable Reads. (Default for MySQL).
* **Serializable:** The strictest. Transactions happen as if they were in a single-file line. Prevents everything, but it's the slowest.
