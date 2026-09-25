# API

## 1. What is an API?

API stands for **Application Programming Interface**.

It defines how different software components communicate with each other.

A common web API flow is:

```text
Client
  ↓
HTTP Request
  ↓
Backend / Server
  ↓
Database
  ↓
Backend / Server
  ↓
HTTP Response
  ↓
Client
```

An API is a general concept. It does not necessarily mean REST.

---

## 2. REST API

REST stands for **Representational State Transfer**.

REST is an architectural style for designing web APIs around **resources** and standard HTTP semantics.

Example resource:

```text
/users
/products
/orders
/incidents
```

Instead of designing URLs around actions:

```text
/getUsers
/createUser
/deleteUser
```

REST-style APIs generally use the resource as the URL and use HTTP methods to describe the operation:

```text
GET    /users
POST   /users
GET    /users/42
PATCH  /users/42
DELETE /users/42
```

---

## 3. API vs REST

```text
API  → General interface for communication between software components.

REST → One architectural style for designing APIs.
```

Therefore:

```text
Every REST API is an API,
but not every API is REST.
```

---

## 4. REST Principles

### Resource-based URLs

URLs represent **resources**, not actions.

```text
/users
/products
/orders/42
```

The HTTP method tells the server what operation is being performed.

---

### Statelessness

Each request should contain the information needed to process it.

The server should not depend on the client's previous request to understand the current request.

For example:

```text
Request 1:
GET /profile
Authorization: Bearer <token>

Request 2:
GET /orders
Authorization: Bearer <token>
```

The second request should contain the necessary context itself.

### Important clarification

Statelessness does **not** mean the server cannot store data.

A server can still persist:

```text
Users
Orders
Messages
Incidents
Database records
```

The important point is that the server should not rely on temporary client-specific state from a previous request in order to process the current request.

---

### Client-server separation

The client and server have separate responsibilities.

```text
Client
→ UI
→ User interaction
→ Sending requests

Server
→ Business logic
→ Authentication
→ Database operations
→ Returning responses
```

They communicate through the API.

---

## 5. HTTP Request and Response

A client sends a request containing things such as:

```text
HTTP method
URL
Headers
Body
```

Example:

```http
POST /users
Content-Type: application/json
```

```json
{
  "name": "Rudraksh",
  "email": "r@example.com"
}
```

The server processes the request and returns a response containing things such as:

```text
Status code
Headers
Body
```

Example:

```http
201 Created
Content-Type: application/json
```

```json
{
  "id": 42,
  "name": "Rudraksh",
  "email": "r@example.com"
}
```

---

# 6. Common HTTP Status Codes

## 200 — OK

The request succeeded.

Common for:

```text
GET requests
Successful updates
Successful operations that return a response
```

---

## 201 — Created

A new resource was successfully created.

Common with:

```text
POST /users
POST /orders
```

---

## 400 — Bad Request

The server cannot process the request because the request itself is invalid.

Examples:

```text
Missing required field
Invalid input
Malformed request
```

---

## 401 — Unauthorized

The request does not contain valid authentication credentials.

Example:

```text
No token
Invalid token
Expired authentication
```

Think:

```text
"Who are you?"
```

---

## 403 — Forbidden

The user is authenticated, but does not have permission to perform the requested action.

Example:

```text
User is logged in
      ↓
Tries to access an admin-only operation
      ↓
403 Forbidden
```

Think:

```text
"I know who you are, but you're not allowed to do this."
```

---

## 404 — Not Found

The requested resource does not exist.

Example:

```http
GET /users/999999
```

when that user does not exist.

---

## 500 — Internal Server Error

A server-side error occurred while processing the request.

It generally indicates a problem on the server rather than an invalid client request.

---

# 7. PUT vs PATCH

Both are used to update an existing resource.

## PUT

PUT is generally intended for **replacing the resource representation**.

Example:

```http
PUT /users/42
```

```json
{
  "name": "Rudraksh",
  "email": "new@example.com",
  "age": 21
}
```

Conceptually:

```text
Old resource
    ↓
Replace
    ↓
New representation
```

---

## PATCH

PATCH is intended for **partial modification** of a resource.

Example:

```http
PATCH /users/42
```

```json
{
  "email": "new@example.com"
}
```

Only the specified field needs to change.

Conceptually:

```text
Old resource
    ↓
Apply specified changes
    ↓
Updated resource
```

Remember:

```text
PUT   → replacement
PATCH → partial modification
```

### Interview-ready distinction

> PUT is generally intended for replacing a resource representation, while PATCH is intended for partially modifying a resource.

---

# 8. Idempotency

An operation is **idempotent** when making the same request multiple times has the same intended final effect as making it once.

### PUT

PUT is generally idempotent.

Example:

```http
PUT /users/42
```

```json
{
  "name": "Alice"
}
```

Sending the same request multiple times still leaves the resource with:

```json
{
  "name": "Alice"
}
```

---

### PATCH

PATCH can be idempotent, but is **not inherently required to be**.

For example, this kind of patch:

```json
{
  "age": 21
}
```

can be idempotent.

But an operation such as:

```text
increment age by 1
```

would not be idempotent because repeating the same request keeps changing the value.

Therefore, avoid memorizing:

```text
PATCH = non-idempotent
```

The safer understanding is:

```text
PUT   → generally idempotent
PATCH → may or may not be idempotent depending on the operation
```

---

# 9. Core Interview Mental Model

When thinking about a web API:

```text
Client
   ↓
HTTP Request
   ↓
Route / Endpoint
   ↓
Backend Logic
   ↓
Database
   ↓
HTTP Response
   ↓
Client
```

For REST-style APIs:

```text
URL       → identifies the resource
HTTP verb  → identifies the operation
Status    → communicates the result
Body      → carries data
```

---

# 10. Key Things to Remember

```text
API
→ General interface for communication

REST
→ Architectural style for APIs

Stateless
→ Each request contains the context needed to process it

200
→ Success

201
→ Resource created

400
→ Invalid request

401
→ Authentication required/invalid

403
→ Authenticated but not permitted

404
→ Resource not found

500
→ Server error

PUT
→ Replace resource representation

PATCH
→ Partially modify resource

Idempotent
→ Repeating the same request produces the same intended final effect