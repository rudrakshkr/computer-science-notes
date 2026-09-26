# Express.js Interview Notes

## 1. What is Express.js?

Express.js is a lightweight web framework built on top of Node.js.

Node.js provides the runtime and low-level capabilities such as the built-in `http` module. Express provides higher-level abstractions that make web-server and API development easier.

Express commonly provides:

- Routing
- Middleware
- Request/response handling
- Error handling
- Request body parsing
- Router organization

Mental model:

```text
Node.js
→ Runtime

Express
→ Web framework built on Node.js

Express application
→ Middleware → Routes → Handlers → Response
```

---

# 2. Express Application

Create an Express application:

```js
const express = require("express");

const app = express();
```

`express()` returns an Express application instance.

Common methods:

```js
app.use(...)
app.get(...)
app.post(...)
app.patch(...)
app.delete(...)
app.listen(...)
```

Start the server:

```js
app.listen(3000);
```

---

# 3. Request-Response Cycle

Typical flow:

```text
Client
  ↓
HTTP Request
  ↓
Express application
  ↓
Middleware
  ↓
Route matching
  ↓
Route handler
  ↓
Response
  ↓
Client
```

Middleware can run before the final route handler.

---

# 4. Middleware

Middleware is a function that runs during the request-response cycle.

Typical signature:

```js
(req, res, next)
```

It can:

- Inspect the request
- Modify the request
- Modify the response
- End the request
- Pass control to the next middleware/handler

Example:

```js
app.use((req, res, next) => {
    console.log(req.method, req.url);
    next();
});
```

---

# 5. `next()`

`next()` passes control to the next matching middleware or handler.

Example:

```js
app.use((req, res, next) => {
    console.log("Middleware 1");
    next();
});

app.use((req, res, next) => {
    console.log("Middleware 2");
    next();
});

app.get("/", (req, res) => {
    res.send("Hello");
});
```

Flow:

```text
Request
  ↓
Middleware 1
  ↓ next()
Middleware 2
  ↓ next()
Route handler
  ↓
Response
```

A middleware does not have to call `next()` if it ends the request itself:

```js
app.use((req, res) => {
    res.status(401).json({
        error: "Unauthorized"
    });
});
```

---

# 6. Error Passing with `next()`

Passing an error to `next()` tells Express to enter error-handling middleware.

```js
next(error);
```

Example:

```js
app.use((req, res, next) => {
    try {
        // something fails
    } catch (error) {
        next(error);
    }
});
```

---

# 7. `req` — Request Object

`req` contains information coming from the client.

Common properties:

```js
req.params
req.query
req.body
req.headers
req.method
req.url
```

---

# 8. `res` — Response Object

`res` is used to construct/send the response.

Common methods:

```js
res.send()
res.json()
res.status()
res.redirect()
```

Example:

```js
res.status(404).json({
    error: "User not found"
});
```

Important distinction:

```text
req  → incoming request
res  → outgoing response
next → continue middleware chain
```

---

# 9. Route Parameters — `req.params`

Route parameters are dynamic parts of the URL.

Example:

```js
app.get("/users/:id", (req, res) => {
    console.log(req.params.id);
});
```

Request:

```text
GET /users/123
```

Then:

```js
req.params.id
```

is:

```text
"123"
```

Important:

```js
typeof req.params.id
```

is:

```text
"string"
```

even when the value looks numeric.

---

# 10. Query Parameters — `req.query`

Query parameters appear after `?` in the URL.

Example:

```text
GET /products?category=shoes&sort=price
```

Access them with:

```js
req.query.category
req.query.sort
```

Result:

```text
"shoes"
"price"
```

Query parameters are commonly used for:

```text
Filtering
Sorting
Pagination
Searching
Optional parameters
```

---

# 11. Request Body — `req.body`

The request body contains data sent with the request.

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

Access it with:

```js
req.body.name
req.body.email
```

The body must be parsed before Express can provide parsed JSON through `req.body`.

---

# 12. `express.json()`

For JSON request bodies:

```js
app.use(express.json());
```

This middleware parses incoming JSON and makes it available through:

```js
req.body
```

Example:

```js
const express = require("express");

const app = express();

app.use(express.json());

app.post("/users", (req, res) => {
    console.log(req.body);
    res.json(req.body);
});
```

Without the JSON parser, `req.body` may be unavailable/undefined for JSON requests.

---

# 13. Routing

A route consists of:

```text
HTTP method
+
path
+
handler
```

Example:

```js
app.get("/users", (req, res) => {
    res.json([]);
});
```

Here:

```text
GET        → HTTP method
/users     → path
callback   → route handler
```

Common route methods:

```js
app.get()
app.post()
app.put()
app.patch()
app.delete()
```

---

# 14. Route Matching

Express matches a request against registered routes.

Example:

```js
app.get("/users", handler1);

app.get("/users/:id", handler2);
```

Request:

```text
GET /users/123
```

matches:

```text
/users/:id
```

because `123` satisfies the dynamic `:id` segment.

---

# 15. Route Order

Express checks matching middleware/routes in registration order.

Example:

```js
app.get("/users/:id", handler1);

app.get("/users/123", handler2);
```

Request:

```text
GET /users/123
```

can match the first route first.

If `handler1` sends a response and does not pass control onward, `handler2` will not execute.

Therefore route order matters.

---

# 16. `req.params` vs `req.query` vs `req.body`

Example:

```text
POST /users/123?notify=true
```

with:

```json
{
    "name": "Rudraksh"
}
```

Use:

```js
req.params.id
req.query.notify
req.body.name
```

Mental model:

```text
params → route-specific resource identifier

query → optional URL parameters for filtering/sorting/etc.

body → submitted request data
```

---

# 17. Application-Level Middleware

Application-level middleware is attached to the Express application.

Example:

```js
app.use((req, res, next) => {
    console.log("Every request");
    next();
});
```

It can affect many routes.

---

# 18. Route-Level Middleware

Middleware can also be attached to a specific route.

```js
function auth(req, res, next) {
    // authentication
    next();
}

app.get("/profile", auth, (req, res) => {
    res.json({
        message: "Profile"
    });
});
```

Only requests reaching `/profile` use this middleware.

---

# 19. Path-Specific Middleware with `app.use()`

`app.use()` can optionally receive a path.

```js
app.use("/api", middleware);
```

The middleware applies to matching paths under `/api`.

Example:

```text
/api/users
/api/products
/api/orders
```

---

# 20. `app.use()` with No Path

```js
app.use(middleware);
```

This applies the middleware broadly to requests handled by the application.

Example:

```js
app.use((req, res, next) => {
    console.log("Request received");
    next();
});
```

---

# 21. Authentication Middleware

Authentication middleware checks whether the client is authenticated.

Example:

```js
function auth(req, res, next) {
    const token = req.headers.authorization;

    if (!token) {
        return res.status(401).json({
            error: "Unauthorized"
        });
    }

    req.user = verifyToken(token);

    next();
}
```

Then:

```js
app.get("/profile", auth, (req, res) => {
    res.json(req.user);
});
```

Flow:

```text
Request
  ↓
Auth middleware
  ↓
Valid credentials?
  ↓
Yes → next()
No  → 401 response
```

---

# 22. Authentication vs Authorization

### Authentication

Answers:

```text
"Who are you?"
```

Examples:

```text
Login
JWT validation
Session validation
```

### Authorization

Answers:

```text
"Are you allowed to perform this action?"
```

Example:

```text
Authenticated user
      ↓
Admin-only endpoint
      ↓
Not an admin
      ↓
403 Forbidden
```

---

# 23. Express Router

As an application grows, keeping every route in one file becomes difficult.

`express.Router()` allows routes to be grouped into modular routers.

Example:

```js
const express = require("express");

const router = express.Router();

router.get("/", (req, res) => {
    res.json([]);
});

router.get("/:id", (req, res) => {
    res.json({
        id: req.params.id
    });
});

module.exports = router;
```

Mount it:

```js
const usersRouter = require("./routes/users");

app.use("/users", usersRouter);
```

Now:

```text
GET /users
GET /users/123
```

are handled by the router.

Mental model:

```text
Express app
   ↓
Router
   ↓
Related routes
```

---

# 24. Why Use Routers?

Routers improve:

```text
Organization
Maintainability
Separation of concerns
Scalability
```

Example structure:

```text
routes/
├── users.js
├── products.js
└── orders.js
```

Then:

```js
app.use("/users", usersRouter);
app.use("/products", productsRouter);
app.use("/orders", ordersRouter);
```

---

# 25. Error-Handling Middleware

Express error-handling middleware has **four parameters**:

```js
(err, req, res, next)
```

Example:

```js
app.use((err, req, res, next) => {
    console.error(err);

    res.status(500).json({
        error: "Internal Server Error"
    });
});
```

The four parameters are important because they distinguish it from normal middleware.

---

# 26. Error Middleware Placement

Error-handling middleware is generally registered **after the routes and other middleware**.

Typical flow:

```text
Request
  ↓
Middleware
  ↓
Route
  ↓
Error occurs
  ↓
next(error)
  ↓
Error middleware
  ↓
Response
```

Example:

```js
app.get("/users", (req, res, next) => {
    try {
        throw new Error("Database failed");
    } catch (error) {
        next(error);
    }
});

app.use((err, req, res, next) => {
    res.status(500).json({
        error: err.message
    });
});
```

---

# 27. Async Route Handlers

Many Express routes perform asynchronous operations:

```js
app.get("/users", async (req, res) => {
    const users = await getUsers();

    res.json(users);
});
```

Async code can fail, so errors need to reach error-handling middleware.

A common approach is wrapping async handlers:

```js
const asyncHandler = (fn) => (req, res, next) => {
    Promise
        .resolve(fn(req, res, next))
        .catch(next);
};
```

Usage:

```js
app.get("/users", asyncHandler(async (req, res) => {
    const users = await getUsers();
    res.json(users);
}));
```

The important interview idea is:

```text
Async error
    ↓
next(error)
    ↓
Error-handling middleware
```

---

# 28. Request Validation

Never assume client input is valid.

Validate things such as:

```text
Required fields
Data types
Allowed values
String lengths
Email format
IDs
Business rules
```

Example:

```js
app.post("/users", (req, res, next) => {
    const { name, email } = req.body;

    if (!name || !email) {
        return res.status(400).json({
            error: "name and email are required"
        });
    }

    next();
});
```

Validation can be implemented with dedicated libraries as well.

---

# 29. CORS

CORS stands for **Cross-Origin Resource Sharing**.

Browsers restrict certain cross-origin requests for security reasons.

An API may need to explicitly allow requests from another origin.

Common Express setup:

```js
const cors = require("cors");

app.use(cors());
```

Or with a specific origin:

```js
app.use(cors({
    origin: "https://example.com"
}));
```

Important:

```text
CORS is primarily a browser-enforced cross-origin mechanism.
```

It is not an authentication system.

---

# 30. Common Middleware Examples

Typical Express applications use middleware for:

```text
express.json()
Authentication
Authorization
Logging
Validation
CORS
Rate limiting
Error handling
```

---

# 31. Response Handling

Common:

```js
res.send()
res.json()
res.status()
```

Example:

```js
res.status(201).json({
    id: 42
});
```

Avoid trying to send multiple responses for the same request.

Bad pattern:

```js
if (!user) {
    res.status(404).json({
        error: "Not found"
    });
}

res.json(user);
```

Better:

```js
if (!user) {
    return res.status(404).json({
        error: "Not found"
    });
}

res.json(user);
```

The `return` prevents the handler from continuing.

---

# 32. Common Express Architecture

A larger Express application can be organized as:

```text
Client
  ↓
Route
  ↓
Middleware
  ↓
Controller
  ↓
Service
  ↓
Database
```

Possible project structure:

```text
src/
├── routes/
├── controllers/
├── services/
├── middleware/
├── models/
└── app.js
```

The exact structure depends on the application.

---

# 33. Route vs Controller

A route defines:

```text
Which HTTP method/path should match?
```

Example:

```js
router.get("/users", getUsers);
```

The controller handles:

```text
What should happen after the route matches?
```

Example:

```js
async function getUsers(req, res) {
    const users = await userService.getUsers();
    res.json(users);
}
```

Mental model:

```text
Route
→ Where?

Controller
→ What?

Service
→ Business logic

Database layer
→ Data persistence
```

---

# 34. Middleware Execution Order

Middleware executes according to registration/matching order.

Example:

```js
app.use(middleware1);
app.use(middleware2);

app.get("/", handler);
```

Flow:

```text
middleware1
   ↓
middleware2
   ↓
handler
```

If `middleware1` does not call `next()` and does not send a response, the request can get stuck.

---

# 35. Important `next()` Scenarios

### Continue normally

```js
next();
```

### Pass an error

```js
next(error);
```

### Stop request

Send a response:

```js
return res.status(401).json({
    error: "Unauthorized"
});
```

---

# 36. Common Interview Questions

### What is Express?

> Express is a web framework built on Node.js that provides abstractions for routing, middleware, and HTTP request/response handling.

### What is middleware?

> Middleware is a function that runs during the request-response cycle and can inspect or modify the request/response, terminate the request, or pass control using `next()`.

### What does `next()` do?

> It passes control to the next matching middleware or handler. `next(error)` passes control to error-handling middleware.

### Difference between `req.params` and `req.query`?

```text
req.params → values defined by the route pattern

req.query  → values provided in the URL query string
```

### Difference between `req.body` and `req.query`?

```text
req.body  → data sent in the request body

req.query → data sent in the URL query string
```

### What is `express.json()`?

> Middleware that parses incoming JSON request bodies and exposes the parsed data through `req.body`.

### What is `express.Router()`?

> A modular router used to group and organize related routes.

### How does Express handle errors?

> Errors can be passed using `next(error)` and handled by middleware with the signature `(err, req, res, next)`.

---

# 37. Important Pitfalls

### Forgetting `next()`

```js
app.use((req, res, next) => {
    console.log("hello");
    // request does not continue
});
```

### Incorrect error middleware signature

Normal middleware:

```js
(req, res, next)
```

Error middleware:

```js
(err, req, res, next)
```

### Ignoring input validation

Never blindly trust:

```js
req.params
req.query
req.body
```

Validate and sanitize input according to the application's requirements.

---

# 38. Express Mental Model

Remember the complete flow:

```text
Request
   ↓
Global middleware
   ↓
Path-specific middleware
   ↓
Route matching
   ↓
Route middleware
   ↓
Controller / handler
   ↓
Service / business logic
   ↓
Database
   ↓
Response
```

Core objects:

```text
req  → incoming data

res  → outgoing response

next → continue middleware chain
```

Core request data:

```text
req.params → route parameters

req.query  → query parameters

req.body   → request body

req.headers → HTTP headers
```

Core architecture:

```text
Express
├── Middleware
├── Routing
├── Request/Response
├── Routers
└── Error handling
```