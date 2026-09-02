# LeetCode 143 — Reorder List

## Problem

Given a linked list:

`L0 → L1 → L2 → ... → Ln`

Reorder it into:

`L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`

Example:

`1 → 2 → 3 → 4 → 5 → 6`

becomes:

`1 → 6 → 2 → 5 → 3 → 4`

The list must be modified **in place**.

## Core Approach

The solution combines three linked-list techniques:

1. **Slow/Fast pointers** → find the middle
2. **Reverse a linked list** → reverse the second half
3. **Pointer manipulation** → merge the two halves alternately

### Step 1 — Find the middle

Use `slow` and `fast`.

```js
while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
}
```

For:

`1 → 2 → 3 → 4 → 5 → 6`

`slow` ends at `4`.

This means the two parts are:

```text
1 → 2 → 3 → 4

5 → 6
```

The first half can be one node longer. The halves do **not** need to be equal.

### Step 2 — Split the list

Save the start of the second half before cutting the connection:

```js
let second = slow.next;
slow.next = null;
```

Now:

```text
left:   1 → 2 → 3 → 4

second: 5 → 6
```

### Step 3 — Reverse the second half

Reverse `second` using the standard 3-pointer technique.

```js
let curr = second;
let prev = null;

while (curr !== null) {
    let next = curr.next;
    curr.next = prev;

    prev = curr;
    curr = next;
}
```

Now:

```text
left:  1 → 2 → 3 → 4
right: 6 → 5
```

`prev` points to the head of the reversed second half.

### Step 4 — Merge alternately

Start with:

```js
let left = head;
let right = prev;
```

At every iteration, **save both next pointers before changing any links**.

```js
while (right) {
    let nextLeft = left.next;
    let nextRight = right.next;

    left.next = right;
    right.next = nextLeft;

    left = nextLeft;
    right = nextRight;
}
```

The saving step is essential because changing `left.next` or `right.next` overwrites the original connection.

For example:

```text
left  → 1 → 2 → 3 → 4
right → 6 → 5
```

Save:

```text
nextLeft  → 2
nextRight → 5
```

Then connect:

```text
1 → 6 → 2
```

and move:

```text
left  → 2
right → 5
```

Continue until `right` becomes `null`.

Final result:

```text
1 → 6 → 2 → 5 → 3 → 4
```

## Final Code

```js
reorderList(head) {
    let slow = head;
    let fast = head;

    // Find the middle
    while (fast !== null && fast.next !== null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Split the list
    let second = slow.next;
    slow.next = null;

    // Reverse the second half
    let curr = second;
    let prev = null;

    while (curr !== null) {
        let next = curr.next;
        curr.next = prev;

        prev = curr;
        curr = next;
    }

    // Merge the two halves alternately
    let left = head;
    let right = prev;

    while (right) {
        let nextLeft = left.next;
        let nextRight = right.next;

        left.next = right;
        right.next = nextLeft;

        left = nextLeft;
        right = nextRight;
    }
}
```

## Complexity

- **Time:** `O(n)`
  - Finding the middle: `O(n)`
  - Reversing: `O(n)`
  - Merging: `O(n)`

- **Extra Space:** `O(1)`

## Key Takeaway

The important linked-list pattern here is:

```text
Find middle
    ↓
Split
    ↓
Reverse second half
    ↓
Merge alternately
```

When modifying pointers, **always save the old `next` pointer before overwriting it**.