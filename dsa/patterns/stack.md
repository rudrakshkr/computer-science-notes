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

## Maintaining Auxiliary State ⭐

Sometimes the Stack needs to store **more information than just the original value**.

Instead of:

```js
stack.push(val)
```

we can store an object:

```js
stack.push({
    val: val,
    min: newMin
})
```

This allows every Stack element to remember some useful information about the Stack **up to that point**.

For example:

```text
push(5)

{ val: 5, min: 5 }

push(3)

{ val: 3, min: 3 }

push(7)

{ val: 7, min: 3 }

push(2)

{ val: 2, min: 2 }
```

Now the top element contains the current minimum.

### Key Idea

> **Store information alongside each element so that future operations can be performed in O(1).**

This is useful when a problem asks for information about the current Stack state, such as:

- Minimum
- Maximum
- Previous state
- Running information
- Other values that would otherwise require scanning the Stack

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

---

## Key Insight

If something must be processed in **reverse order of insertion**, think:

```text
Last In
   ↓
First Out
   ↓
STACK
```

If the problem also asks for some information about the current Stack state, consider:

```text
Stack element
      ↓
value + auxiliary information
```