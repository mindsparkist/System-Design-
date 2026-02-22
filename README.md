API Design is about creating a "user interface" for developers. A well-designed API should be intuitive, predictable, and scalable. Since you are documenting this for your repository, focusing on **RESTful patterns** is the best place to start.

---

### 1. Designing RESTful Endpoints

The golden rule of REST is: **Use Nouns, not Verbs.** The HTTP method (GET, POST, etc.) defines the action; the URL defines the resource.

| Action | Bad (Verb-based) | Good (Noun-based) |
| --- | --- | --- |
| **Create a User** | `/createUser` | `POST /users` |
| **Get all Users** | `/getAllUsers` | `GET /users` |
| **Get User #10** | `/getUser?id=10` | `GET /users/10` |
| **Delete User #10** | `/deleteUser/10` | `DELETE /users/10` |

**Nesting Resources:**
If you want to get all posts by a specific user:
`GET /users/10/posts`
This shows a clear hierarchy: User  Posts.

---

### 2. Standard CRUD Mapping

Every resource should support the basic CRUD (Create, Read, Update, Delete) operations using the correct status codes.

1. **Create:** `POST /users`  Returns `201 Created`.
2. **Read:** `GET /users/10`  Returns `200 OK`.
3. **Update:** * `PUT /users/10` (Replaces the whole user).
* `PATCH /users/10` (Updates only specific fields, like just the email).


4. **Delete:** `DELETE /users/10`  Returns `204 No Content`.

---

### 3. Filtering, Sorting, and Limits

When dealing with large datasets (like thousands of logs at Deloitte), you must provide ways to slice the data.

* **Filtering:** `GET /users?role=admin`
* **Sorting:** `GET /users?sort=created_at:desc`
* **Limiting (Pagination):** `GET /users?limit=20&page=1`
* **Limit:** How many items to return.
* **Offset/Page:** Where to start.



---

### 4. How to Read & Write API Docs

Good documentation is the difference between a used API and a dead one.

#### How to Read Docs:

1. **Authentication:** Find out if you need an `API_KEY` or `Bearer Token`.
2. **Base URL:** The starting point (e.g., `https://api.example.com/v1`).
3. **Endpoints & Methods:** What can I call?
4. **Request/Response Examples:** Look for the JSON structure to see what data you'll get back.

#### How to Write Docs:

The industry standard is **OpenAPI (formerly Swagger)**. It allows you to write a YAML/JSON file that generates an interactive UI.

**Key sections to include in your docs:**

* **Description:** What does this endpoint actually do?
* **Path Parameters:** (e.g., `id` in `/users/{id}`).
* **Query Parameters:** (e.g., `limit`, `sort`).
* **Header Requirements:** (e.g., `Content-Type: application/json`).
* **Status Codes:** List what happens on success AND failure (e.g., what does a 403 error look like for this endpoint?).

---

### 5. API Versioning

Never break your users' code! If you make a massive change, version your API:

* `https://api.example.com/v1/users`
* `https://api.example.com/v2/users`

> **Pro Tip:** In your System Design repository, you might want to mention **Idempotency**. `GET`, `PUT`, and `DELETE` should be idempotent (calling them 10 times has the same effect as calling them once). `POST` is NOT idempotent (calling it 10 times creates 10 users).
