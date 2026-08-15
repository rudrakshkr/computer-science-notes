# Stack

## 💡 Core Idea

A **Stack** follows the **LIFO** principle:

> **Last In, First Out**

The element added most recently is the first element removed.

Example:

```text
push A
push B
push C

Stack:

C  ← top
B
A
```

Calling `pop()` removes `C` first.

---

## When to Use a Stack

Think about a Stack when the problem requires:

- Processing the **most recently added element first**
- Matching opening and closing elements
- Reversing or undoing operations
- Tracking nested structures
- Evaluating expressions
- Maintaining a history of previous states
- Backtracking through recently encountered elements

### Key Recognition Question ⭐

Ask:

> **Do I need to process the most recently added element before the older elements?**

If yes, a Stack may be the correct data structure.

---

## Basic Stack Operations

In JavaScript, an array can be used as a Stack.

```js
const stack = []
```

### `push()`

Adds an element to the top.

```js
stack.push("A")
stack.push("B")
stack.push("C")
```

Stack:

```text
C  ← top
B
A
```

### `pop()`

Removes and returns the top element.

```js
stack.pop()
```

Returns:

```text
C
```

Stack becomes:

```text
B  ← top
A
```

### `at(-1)`

Gets the top element without removing it.

```js
stack.at(-1)
```

---

## Basic Pattern

```js
const stack = []

for (const element of input) {

    if (/* element should be added */) {
        stack.push(element)
    } else {
        const top = stack.at(-1)

        if (/* top matches element */) {
            stack.pop()
        } else {
            return false
        }
    }
}
```

The exact condition depends on the problem.

---

## Complexity

For a Stack implemented using a JavaScript array:

```text
push() → O(1)
pop()  → O(1)
peek   → O(1)
```

If there are `n` elements:

```text
Time:  O(n)
Space: O(n)
```

## Key Insight

If something must be processed in **reverse order of insertion**, think:

```text
Last In
   ↓
First Out
   ↓
STACK
```