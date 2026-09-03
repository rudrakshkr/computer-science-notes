# LeetCode 138 — Copy List with Random Pointer

## Problem

Each linked-list node contains:

- `val`
- `next`
- `random`

The `next` pointer points to the next node, while `random` can point to **any node in the list or `null`**.

The goal is to create a **deep copy** of the entire linked list.

A deep copy means:

> Every node in the copied list must be a new node. No `next` or `random` pointer in the copied list should point to a node from the original list.

---

## Core Idea

Use a **HashMap (`Map`)** to maintain the relationship:

```text
original node → copied node
```

Example:

```text
Original nodes:
A → B → C

Copied nodes:
X → Y → Z

Map:
A → X
B → Y
C → Z
```

The Map is necessary because `random` can point to any node.

A `Set` is not enough because we don't just need to know whether a node exists. We need to know **which copied node corresponds to each original node**.

---

## Two-Pass Approach

### Pass 1 — Create all copied nodes

Traverse the original list and create a new node for every original node.

Store the mapping:

```js
map.set(curr, new Node(curr.val));
```

After this pass:

```text
original node → copied node
```

Every copied node exists, but its `next` and `random` pointers are not connected yet.

---

### Pass 2 — Connect `next` and `random`

Traverse the original list again.

For each original node:

```js
const copy = map.get(curr);
```

This gets the corresponding copied node.

Then:

```js
copy.next = curr.next ? map.get(curr.next) : null;
copy.random = curr.random ? map.get(curr.random) : null;
```

The important idea is:

```text
curr        = original node
copy        = corresponding copied node

curr.next   = original next node
map.get()   = copied version of that node
```

So if:

```text
A.next → B
```

then:

```text
map.get(B) → B's copy
```

and therefore:

```text
A's copy.next → B's copy
```

The same idea works for `random`.

---

## Why We Need the Map

Suppose:

```text
A.random → C
```

We cannot do:

```js
copy.random = curr.random;
```

because that would make the copied node point to the **original** `C`.

Instead:

```js
copy.random = map.get(curr.random);
```

Now the copied node points to **C's copy**.

---

## Edge Case

If the input list is empty:

```js
head === null
```

return:

```js
null
```

So start with:

```js
if (head === null) return null;
```

---

## Final Code

```js
copyRandomList(head) {
    if (head === null) return null;

    const map = new Map();

    // Pass 1: create a copy of every node
    let curr = head;

    while (curr !== null) {
        map.set(curr, new Node(curr.val));
        curr = curr.next;
    }

    // Pass 2: connect next and random pointers
    curr = head;

    while (curr !== null) {
        const copy = map.get(curr);

        copy.next = curr.next ? map.get(curr.next) : null;
        copy.random = curr.random ? map.get(curr.random) : null;

        curr = curr.next;
    }

    return map.get(head);
}
```

## Complexity

- **Time:** `O(n)`
- **Extra Space:** `O(n)`

The `Map` stores one mapping for every node.

## Key Takeaway

For a linked list with arbitrary pointers:

```text
Original node → Copied node
```

Use a `Map` to maintain the correspondence.

The overall pattern is:

```text
Create all copies
       ↓
Store original → copy mappings
       ↓
Use the mappings to connect next/random
```