# Closures

## 💡 What is a Closure?

A closure is created when an inner function remembers and can access the variables from its **lexical (outer) scope**, even after the outer function has finished executing.

---

## 🧠 Why Do Closures Work?

When a function is created, JavaScript stores a reference to its **lexical environment**.

If the inner function is still being used, JavaScript keeps that lexical environment alive instead of removing it from memory.

---

## 📌 Example

```javascript
function outer() {
    let count = 0;

    function inner() {
        count++;
        console.log(count);
    }

    return inner;
}

const counter = outer();

counter(); // 1
counter(); // 2
counter(); // 3
```

### Why?

- `outer()` finishes executing.
- `inner()` still references `count`.
- JavaScript keeps `count` alive.
- Every call updates the **same** variable.

---

## Multiple Closures

```javascript
const counter1 = outer();
const counter2 = outer();
```

Each call to `outer()` creates a **new lexical environment**.

```
counter1
↓

count = 0

-------------------

counter2
↓

count = 0
```

Both counters are completely independent.

---

## Closures Remember Variables, Not Values

Closures **do not** store copies of values.

They store **references to variables (bindings)**.

This is why changes to a variable are visible inside the closure.

---

## Closures + `var`

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}
```

Output

```
3
3
3
```

### Why?

`var` is **function-scoped**.

Only **one** variable `i` exists.

All callbacks reference the **same** variable.

When they execute, the loop has already finished.

```
i = 3
```

---

## Closures + `let`

```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}
```

Output

```
0
1
2
```

### Why?

`let` is **block-scoped**.

JavaScript creates a **new binding** for every loop iteration.

Each callback closes over its own binding.

---

## `var` vs `let`

### `var`

```
One variable

↓

Every callback references it

↓

3
3
3
```

---

### `let`

```
New binding every iteration

↓

Each callback remembers its own binding

↓

0
1
2
```

---

## Real-World Uses

- Event Listeners
- `setTimeout()`
- `setInterval()`
- Data privacy (Encapsulation)
- React Hooks & Callbacks

---

## Interview Tips

✅ A closure remembers its **lexical scope** of a function.

✅ Closures remember **variables (bindings)**, **not values**.

✅ Each call to a function creates a **new lexical environment**.

✅ `var` shares one binding.

✅ `let` creates a new binding for every loop iteration.