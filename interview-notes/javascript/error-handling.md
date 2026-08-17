# JavaScript Error Handling

## `try / catch`

Used to handle errors without letting them propagate unexpectedly.

```js
try {
    // code that may throw an error
} catch (error) {
    // handle the error
}
```

- `try` → contains code that may throw an error.
- `catch` → runs when an error is thrown.

The error object commonly provides:

```js
error.message
error.name
error.stack
```

Example:

```js
try {
    const data = JSON.parse("invalid json")
} catch (error) {
    console.log(error.message)
}
```

---

## Important Behavior

When an error is thrown inside `try`, execution stops at that point.

```js
try {
    console.log("A")
    throw new Error("Oops")
    console.log("B")
} catch (error) {
    console.log("C")
}

console.log("D")
```

Output:

```text
A
C
D
```

`"B"` never executes because the error immediately transfers control to `catch`.

---

## `finally`

`finally` runs regardless of whether an error occurs.

```js
try {
    // code
} catch (error) {
    // handle error
} finally {
    // always runs
}
```

Commonly used for cleanup:

```js
try {
    // use resource
} finally {
    // cleanup resource
}
```

---

# `throw`

`throw` is used to manually create an error.

```js
throw new Error("Something went wrong")
```

Example:

```js
function divide(a, b) {
    if (b === 0) {
        throw new Error("Cannot divide by zero")
    }

    return a / b
}
```

The thrown error can then be handled with `try / catch`:

```js
try {
    divide(10, 0)
} catch (error) {
    console.log(error.message)
}
```

---

## `throw` Stops Execution

```js
function test() {
    console.log("A")

    throw new Error("Oops")

    console.log("B")
}
```

Output:

```text
A
```

`"B"` never executes.

---

# Custom Errors

You can create your own Error classes when different types of errors need to be distinguished.

```js
class ValidationError extends Error {
    constructor(message) {
        super(message)
        this.name = "ValidationError"
    }
}
```

Use it:

```js
throw new ValidationError("Invalid email")
```

Handle it:

```js
try {
    throw new ValidationError("Invalid email")
} catch (error) {
    if (error instanceof ValidationError) {
        console.log("Validation failed")
    }
}
```

### Key Idea

Custom errors are useful when your application needs to distinguish between different error types.

---

# Error Propagation

If an error isn't caught at the current level, it propagates upward to the caller.

```js
function databaseOperation() {
    throw new Error("Database failed")
}

function service() {
    databaseOperation()
}

try {
    service()
} catch (error) {
    console.log(error.message)
}
```

The error travels:

```text
databaseOperation()
        ↓
service()
        ↓
try / catch
```

This is useful because lower-level functions can throw errors while a higher-level function decides how to handle them.

---

# Async Error Handling

Errors from Promises can be handled with `.catch()`:

```js
fetchData()
    .then(data => {
        console.log(data)
    })
    .catch(error => {
        console.error(error)
    })
```

With `async / await`, use `try / catch`:

```js
async function getData() {
    try {
        const response = await fetch("/api/data")
        const data = await response.json()

        return data
    } catch (error) {
        console.error(error)
    }
}
```

### Important

A rejected Promise behaves like an error at the `await` expression.

```js
async function test() {
    try {
        await Promise.reject(new Error("Failed"))
    } catch (error) {
        console.log(error.message)
    }
}
```

Output:

```text
Failed
```

---

# Re-throwing Errors

Sometimes you catch an error only to perform some local handling, then throw it again.

```js
try {
    await saveData()
} catch (error) {
    console.error("Logging error:", error)
    throw error
}
```

The higher-level caller can then handle it.

This is useful when you want to:

- Log the error
- Add context
- Perform cleanup
- Still let another layer handle the failure

---

# `finally` with Async Code

`finally` also works with `async / await`.

```js
async function loadData() {
    try {
        await fetchData()
    } catch (error) {
        console.error(error)
    } finally {
        console.log("Request finished")
    }
}
```

The `finally` block runs whether the operation succeeds or fails.

---

# Common Error Types

JavaScript provides built-in error types:

```text
Error
TypeError
ReferenceError
SyntaxError
RangeError
```

Examples:

### `TypeError`

```js
null.toString()
```

### `ReferenceError`

```js
console.log(unknownVariable)
```

### `SyntaxError`

Invalid JavaScript syntax.

### `RangeError`

A value is outside an allowed range.

You can generally handle all of them through `catch`:

```js
try {
    // code
} catch (error) {
    console.log(error.name)
}
```

---

# Interview Takeaways

1. `try` contains code that may throw.
2. `catch` handles the thrown error.
3. `finally` runs regardless of success or failure.
4. `throw` manually raises an error.
5. Code after a thrown error in the same execution path doesn't run.
6. Errors can propagate upward through function calls.
7. Custom Error classes can represent specific error types.
8. Promise rejections can be handled with `.catch()`.
9. Rejected Promises can be handled with `try / catch` when using `await`.
10. Errors can be re-thrown after logging or additional handling.