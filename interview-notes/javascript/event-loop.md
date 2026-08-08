# Event Loop

## Definition

The **Event Loop** is the mechanism that allows JavaScript to handle asynchronous operations despite JavaScript being single-threaded.

It continuously checks whether the **Call Stack is empty**, then processes queued callbacks according to their priority.

---

## Execution Order

```text
Synchronous Code
       ↓
Microtasks
       ↓
Tasks
```

### Microtasks
- `Promise.then()`
- `await` continuation

### Tasks / Macrotasks
- `setTimeout()`
- `setInterval()`
- DOM events

> After the Call Stack is empty, **all microtasks are processed before the next task**.

---

## `setTimeout`

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

console.log("C");
```

Output:

```text
A → C → B
```

`0ms` does **not** mean immediately.

---

## Promises

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

Output:

```text
A → D → C → B
```

Promise callbacks are **microtasks**, so they run before the timer task.

---

## `async / await`

An `async` function executes synchronously **until `await`**.

```js
async function test() {
    console.log("A");

    await something;

    console.log("B");
}
```

> `B` resumes as a **microtask** after the `await`.
> Async function runs synchronously **until `await`**.  
> Code after `await` resumes as a **microtask**.