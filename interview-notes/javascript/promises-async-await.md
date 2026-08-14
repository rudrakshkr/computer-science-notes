# Promises & Async/Await

## 1. What is a Promise?

A Promise represents the eventual result of an asynchronous operation.

A Promise has three states:

- `pending` — operation hasn't finished
- `fulfilled` — operation completed successfully
- `rejected` — operation failed

State transitions:

```text
pending → fulfilled
pending → rejected
```

Once a Promise settles, its state cannot change.

---

## 2. `resolve()` and `reject()`

```js
const promise = new Promise((resolve, reject) => {
    if (success) {
        resolve("Success")
    } else {
        reject("Something went wrong")
    }
})
```

### `resolve(value)`

Indicates that the operation succeeded and provides its result.

### `reject(error)`

Indicates that the operation failed and provides the reason.

---

## 3. `.then()`, `.catch()`, `.finally()`

```js
promise
    .then((result) => {
        console.log(result)
    })
    .catch((error) => {
        console.error(error)
    })
    .finally(() => {
        console.log("Finished")
    })
```

- `.then()` handles a fulfilled Promise.
- `.catch()` handles a rejected Promise.
- `.finally()` runs after the Promise settles, regardless of success or failure.

---

# Promise Chaining

## 4. `.then()` Returns a New Promise

```js
Promise.resolve(10)
    .then((value) => {
        return value * 2
    })
    .then((value) => {
        console.log(value)
    })
```

Output:

```text
20
```

The value returned from one `.then()` becomes the value received by the next `.then()`.

---

## 5. Returning Another Promise

```js
getUser()
    .then((user) => {
        return getPosts(user.id)
    })
    .then((posts) => {
        console.log(posts)
    })
```

Returning a Promise causes the next `.then()` to wait for that Promise to settle.

Conceptually:

```text
getUser()
   ↓
user
   ↓
getPosts(user.id)
   ↓
posts
   ↓
next .then()
```

### Important Interview Trap

If you don't return the Promise:

```js
getUser()
    .then((user) => {
        getPosts(user.id)
    })
    .then((posts) => {
        console.log(posts)
    })
```

The first `.then()` returns `undefined`.

Therefore the next `.then()` receives:

```js
undefined
```

Correct:

```js
getUser()
    .then((user) => {
        return getPosts(user.id)
    })
```

Or:

```js
getUser()
    .then(user => getPosts(user.id))
```

---

## 6. Error Propagation

A rejection or thrown error in a Promise chain propagates to the appropriate `.catch()`.

```js
getUser()
    .then((user) => {
        return getPosts(user.id)
    })
    .then((posts) => {
        console.log(posts)
    })
    .catch((error) => {
        console.error(error)
    })
```

An error from:

- `getUser()`
- `getPosts()`
- a later `.then()`

can reach the `.catch()`.

---

# Async / Await

## 7. `async`

It is essentially a cleaner syntax for working with Promises.

An `async` function always returns a Promise.

```js
async function getUser() {
    return "Rudraksh"
}
```

Conceptually similar to:

```js
function getUser() {
    return Promise.resolve("Rudraksh")
}
```

Therefore:

```js
const result = getUser()
```

gives a Promise, not the string directly.

---

## 8. `await`

`await` waits for a Promise's result inside an `async` function.

```js
async function example() {
    const result = await Promise.resolve(42)

    console.log(result)
}
```

Output:

```text
42
```

Important:

> `await` pauses the execution of that async function; it does not block the JavaScript thread.

---

## 9. `async` + `return`

```js
async function test() {
    const value = await Promise.resolve(10)

    return value * 2
}

const result = test()
```

`result` is:

```text
Promise<20>
```

not:

```text
20
```

Because `async` guarantees that the function returns a Promise.

To get `20`:

```js
const result = await test()
```

or:

```js
test().then(result => {
    console.log(result)
})
```

---

# Promise Combinators

## 10. `Promise.all()`

Used when multiple independent Promises all need to succeed.

```js
const [user, posts, comments] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchComments(),
])
```

The operations can proceed concurrently.

If all fulfill:

```js
["user", "posts", "comments"]
```

The results are returned in the same order as the input, regardless of which Promise finishes first.

### Failure

If any Promise rejects:

```text
Promise.all() → rejected
```

### Timing

For independent operations:

```text
Time ≈ max(T1, T2, T3)
```

rather than:

```text
T1 + T2 + T3
```

### Use when:

> All operations are required to succeed.

---

# 11. `Promise.allSettled()`

Waits for every Promise to finish, regardless of whether they fulfill or reject.

```js
const results = await Promise.allSettled([
    Promise.resolve("A"),
    Promise.reject("B"),
    Promise.resolve("C"),
])
```

Result:

```js
[
    {
        status: "fulfilled",
        value: "A"
    },
    {
        status: "rejected",
        reason: "B"
    },
    {
        status: "fulfilled",
        value: "C"
    }
]
```

Important:

```text
fulfilled → { status, value }
rejected  → { status, reason }
```

`Promise.allSettled()` itself fulfills once all Promises have settled.

### Use when:

> You want the outcome of every operation, even if some fail.

---

# 12. `Promise.race()`

Returns the result of the first Promise to settle.

"Settle" means:

- fulfilled
- rejected

Example:

```js
const result = await Promise.race([
    fetchUser(),
    fetchPosts(),
])
```

If `fetchPosts()` settles first, its result wins.

If the first Promise to settle rejects, `Promise.race()` rejects.

```text
Promise.race()
      ↓
first to SETTLE
      ↓
success OR failure
```

Important:

> `Promise.race()` does not cancel the other Promises.

---

# 13. `Promise.any()`

Returns the result of the first Promise to fulfill.

```js
const result = await Promise.any([
    server1(),
    server2(),
    server3(),
])
```

Rejected Promises are ignored while waiting for a successful one.

Example:

```text
server1 → ❌
server2 → ✅  ← result
server3 → ✅
```

If all Promises reject, `Promise.any()` rejects with an `AggregateError`.

Mental model:

```text
Promise.any()
      ↓
first to FULFILL
```

---

# Promise Combinator Summary

| Method | Behavior |
|---|---|
| `Promise.all()` | All must fulfill; one rejection rejects the whole operation |
| `Promise.allSettled()` | Waits for everything and reports every outcome |
| `Promise.race()` | First Promise to settle wins |
| `Promise.any()` | First Promise to fulfill wins |

Easy way to remember:

```text
all          → everyone must succeed
allSettled   → I want everyone's result
race         → whoever settles first
any          → whoever succeeds first
```

---

# Practical Example

Suppose a dashboard needs three independent resources:

```js
async function loadDashboard() {
    const user = await fetchUser()
    const projects = await fetchProjects()
    const notifications = await fetchNotifications()

    return {
        user,
        projects,
        notifications
    }
}
```

This waits for each request before starting the next.

Better:

```js
async function loadDashboard() {
    const [user, projects, notifications] = await Promise.all([
        fetchUser(),
        fetchProjects(),
        fetchNotifications(),
    ])

    return {
        user,
        projects,
        notifications
    }
}
```

Because the requests are independent, they can proceed concurrently.

---

# Interview Takeaways

1. An `async` function always returns a Promise.
2. `await` extracts the fulfilled value of a Promise.
3. Returning a value from `.then()` passes it to the next `.then()`.
4. Returning a Promise from `.then()` makes the next `.then()` wait for it.
5. Forgetting to return a Promise in a chain can cause the next `.then()` to receive `undefined`.
6. `Promise.all()` is useful for independent operations that all need to succeed.
7. `Promise.allSettled()` lets you inspect every result, including failures.
8. `Promise.race()` returns the first Promise to settle.
9. `Promise.any()` returns the first Promise to fulfill.
10. `Promise.all()` preserves input order in its result array.