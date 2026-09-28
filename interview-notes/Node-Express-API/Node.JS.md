# Node.js Interview Notes

## 1. What is Node.js?

Node.js is a **JavaScript runtime environment** that allows JavaScript to run outside the browser.

```text
JavaScript → language
V8         → JavaScript engine
Node.js    → runtime built around V8
```

Node provides APIs for:

```text
File system
HTTP/networking
Processes
Streams
Environment variables
Events
Cryptography
```

Node.js is **not a framework**.

```text
Node.js → runtime
Express → web framework built on Node.js
```

---

## 2. V8 Engine

V8 is the JavaScript engine used by Node.js.

```text
JavaScript
    ↓
V8
    ↓
JavaScript execution
```

Node adds server-side/runtime APIs around V8.

---

## 3. Event-Driven, Non-Blocking Architecture

Node.js uses an **event-driven architecture** and a **non-blocking I/O model**.

Instead of waiting synchronously for I/O:

```text
Start I/O
   ↓
Continue other work
   ↓
I/O completes
   ↓
Handle result
```

Common I/O:

```text
File operations
Database queries
Network requests
HTTP requests
DNS operations
```

This makes Node well suited to **I/O-heavy and highly concurrent applications**.

---

## 4. Event Loop

The event loop coordinates asynchronous work and callback execution.

Simplified:

```text
JavaScript
    ↓
Call Stack
    ↓
Async operation
    ↓
Completion
    ↓
Event Loop
    ↓
Callback executes
```

Node's JavaScript execution on the main event-loop thread is single-threaded, but Node's underlying runtime can use other threads for certain operations.

---

## 5. libuv

`libuv` provides Node's cross-platform asynchronous infrastructure.

It is involved in:

```text
Event loop
Asynchronous I/O
Thread pool
Timers
Networking support
```

Mental model:

```text
JavaScript
    ↓
Node APIs
    ↓
libuv
    ↓
OS / async mechanisms / thread pool
```

---

## 6. Synchronous vs Asynchronous

### Synchronous

Blocks execution until the operation finishes.

```js
const data = fs.readFileSync("file.txt");
```

### Asynchronous

Starts the operation and allows other work to continue.

```js
fs.readFile("file.txt", (err, data) => {
    // runs when operation completes
});
```

For server applications, asynchronous APIs are generally preferred when blocking is unnecessary.

---

# 7. Modules

Node supports two major module systems:

```text
CommonJS
ES Modules
```

### CommonJS

```js
const fs = require("node:fs");

module.exports = something;
```

### ES Modules

```js
import fs from "node:fs";

export default something;
```

Quick comparison:

```text
CommonJS → require(), module.exports
ESM      → import, export
```

---

## 8. Module Configuration

`package.json` can specify:

```json
{
  "type": "module"
}
```

Then `.js` files use ESM semantics by default.

Explicit extensions:

```text
.mjs → ES Modules
.cjs → CommonJS
```

---

## 9. Module Caching

CommonJS modules are cached after being loaded.

Conceptually:

```text
file A ──┐
         ↓
       module
         ↑
file B ──┘
```

Repeated `require()` calls generally reuse the cached module rather than executing it from scratch again.

---

# 10. Built-in Node Modules

Common interview-relevant modules:

```text
fs
path
http
events
crypto
stream
buffer
url
os
util
```

They do not need to be installed from npm.

---

# 11. `fs` — File System

Used for file and directory operations.

Common methods:

```text
readFile()
writeFile()
appendFile()
mkdir()
unlink()
```

Synchronous and asynchronous versions exist:

```text
readFile()       → asynchronous
readFileSync()   → synchronous
```

Promise-based API:

```js
import { readFile } from "node:fs/promises";
```

This works naturally with:

```js
await readFile(...)
```

---

# 12. `path`

Used to safely construct and manipulate file paths.

Important methods:

```text
path.join()
path.resolve()
path.basename()
path.dirname()
path.extname()
```

Example:

```js
path.join("data", "users.json");
```

### `join()` vs `resolve()`

```text
join()
→ combines path segments

resolve()
→ produces an absolute path
```

---

# 13. `__dirname` and `process.cwd()`

In CommonJS:

```text
__dirname
→ directory of the current module

process.cwd()
→ current working directory of the Node process
```

They are not necessarily the same.

---

# 14. `process`

The global `process` object gives information/control over the running process.

Important:

```text
process.env
process.argv
process.cwd()
process.exit()
```

### `process.env`

Reads environment variables:

```js
process.env.DATABASE_URL
```

Environment variables are strings.

```js
process.env.PORT
```

returns something like:

```text
"3000"
```

Convert when necessary:

```js
Number(process.env.PORT)
```

### `process.argv`

Contains command-line arguments.

### `process.cwd()`

Returns the process's current working directory.

### `process.exit()`

Terminates the process.

```text
0       → success convention
non-zero → failure/error convention
```

---

# 15. Environment Variables

Used for configuration and secrets:

```text
PORT
DATABASE_URL
API_KEY
NODE_ENV
```

Access:

```js
process.env.PORT
```

Do not hardcode sensitive credentials in source code.

---

# 16. Timers

Node provides:

```text
setTimeout()
setInterval()
setImmediate()
```

These interact with the event loop.

---

# 17. EventEmitter

Node's `EventEmitter` provides an event-based programming pattern.

Basic operations:

```js
emitter.on("event", handler);
emitter.emit("event");
emitter.once("event", handler);
emitter.off("event", handler);
```

Mental model:

```text
Register listener
      ↓
Event emitted
      ↓
Listener executes
```

### `on()` vs `once()`

```text
on()   → runs every time
once() → runs only once
```

---

# 18. Buffers

A Buffer represents **binary data** in Node.js.

Common uses:

```text
Files
Images
Network data
Streams
Sockets
```

Example:

```js
const buffer = Buffer.from("Hello");
```

Conceptually:

```text
String → text
Buffer → binary data
```

---

# 19. Streams

Streams process data incrementally rather than loading everything into memory at once.

Instead of:

```text
Entire large file
→ load into memory
→ process
```

a stream can do:

```text
chunk 1 → process
chunk 2 → process
chunk 3 → process
...
```

Useful for:

```text
Large files
HTTP data
Video/audio
Data processing
```

### Types

```text
Readable
Writable
Duplex
Transform
```

### Piping

```js
readable.pipe(writable);
```

Connects a data source to a destination.

---

# 20. `http`

Node's built-in `http` module can create an HTTP server without Express.

Conceptually:

```text
http.createServer()
→ request
→ response
```

Express provides higher-level abstractions on top of this functionality.

---

# 21. `crypto`

Node's built-in `crypto` module provides cryptographic functionality such as:

```text
Hashing
Random values
Encryption/decryption primitives
Digital signatures
```

Use established cryptographic APIs rather than inventing custom cryptography.

---

# 22. npm

npm is Node's common package manager.

Install:

```bash
npm install express
```

Development dependency:

```bash
npm install -D eslint
```

Run scripts:

```bash
npm run dev
```

---

# 23. `package.json`

Describes the Node project.

Important fields:

```text
name
version
scripts
dependencies
devDependencies
type
```

Example:

```json
{
  "name": "my-api",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^5.0.0"
  }
}
```

---

# 24. dependencies vs devDependencies

```text
dependencies
→ required by the application at runtime

devDependencies
→ primarily needed during development
```

Examples:

```text
dependencies:
express
prisma

devDependencies:
typescript
eslint
vitest
```

---

# 25. `package-lock.json`

```text
package.json
→ project dependency requirements

package-lock.json
→ exact resolved dependency tree/versions
```

It helps make installations reproducible.

---

# 26. Node.js + Database

Node.js does not provide a specific database.

Applications use drivers/ORMs such as:

```text
pg
Prisma
Drizzle
MongoDB driver
```

Typical flow:

```text
HTTP request
    ↓
Node / Express
    ↓
Business logic
    ↓
Driver / ORM
    ↓
Database
```

---

# 27. Node.js Mental Model

```text
                  Node.js
                     |
          +----------+----------+
          |                     |
         V8                Node APIs
          |              /  |  |  \
   JS execution         fs http path events
          |
      Event Loop
          |
        libuv
          |
   OS / async I/O /
     thread pool
```

---

# 28. Core Interview Takeaways

```text
Node.js
→ JavaScript runtime

V8
→ JavaScript engine

Event loop
→ coordinates asynchronous callback execution

Non-blocking I/O
→ don't wait synchronously for I/O when unnecessary

libuv
→ asynchronous infrastructure used by Node

process.env
→ environment variables

fs
→ file system

path
→ path manipulation

http
→ low-level HTTP server APIs

EventEmitter
→ event-based programming

Buffer
→ binary data

Streams
→ incremental data processing

npm
→ package management

CommonJS
→ require / module.exports

ESM
→ import / export
```