# LeetCode 25 — Reverse Nodes in k-Group

## Problem

Given the head of a linked list, reverse the nodes of the list `k` at a time and return the modified list.

If the number of nodes remaining is less than `k`, leave those nodes unchanged.

### Example

```text
Input:
1 → 2 → 3 → 4 → 5
k = 2

Output:
2 → 1 → 4 → 3 → 5
```

For `k = 3`:

```text
Input:
1 → 2 → 3 → 4 → 5

Output:
3 → 2 → 1 → 4 → 5
```

The last two nodes are unchanged because fewer than `3` nodes remain.

---

# Core Idea

Process the linked list **one group of `k` nodes at a time**.

For every group:

```text
1. Find the kth node.
2. Remember the node after the group.
3. Reverse the current group.
4. Connect the reversed group back to the list.
5. Move to the next group.
```

---

# Why Use a Dummy Node?

Create a node before the actual head:

```text
dummy → 1 → 2 → 3 → 4 → 5
```

This makes it easy to handle the first group because the head can change after reversal.

Initially:

```text
groupPrev
    ↓
dummy → 1 → 2 → 3 → 4 → 5
```

`groupPrev` always points to the node **before the group currently being reversed**.

---

# Important Pointers

For a group:

```text
groupPrev → [1 → 2 → 3] → 4 → 5
                          ↑
                      groupNext
```

We use:

### `groupPrev`

Node immediately before the current group.

### `kth`

The last node of the current group.

```text
groupPrev → [1 → 2 → 3] → 4 → 5
                     ↑
                    kth
```

### `groupNext`

The node immediately after the current group.

```text
groupPrev → [1 → 2 → 3] → 4 → 5
                          ↑
                      groupNext
```

---

# Finding `kth`

```javascript
let kth = groupPrev;

for (let i = 0; i < k; i++) {
    kth = kth.next;

    if (kth === null) {
        return dummy.next;
    }
}
```

If we cannot find `k` nodes, fewer than `k` nodes remain, so we stop.

---

# Saving `groupNext`

```javascript
const groupNext = kth.next;
```

Example:

```text
[1 → 2 → 3] → 4 → 5
         ↑    ↑
        kth  groupNext
```

If `k = 3`:

```text
groupNext = 4
```

`groupNext` acts as the **boundary** telling us where the current group ends.

---

# Reversing the Group

This is the most important part.

```javascript
let prev = groupNext;
let curr = groupPrev.next;

while (curr !== groupNext) {
    const nextNode = curr.next;

    curr.next = prev;
    prev = curr;
    curr = nextNode;
}
```

---

## Why `prev = groupNext`?

Normally, when reversing an entire linked list, we start with:

```javascript
prev = null;
```

But here we are reversing only part of the list.

Example:

```text
1 → 2 → 3 → 4
```

Suppose we only reverse:

```text
1 → 2 → 3
```

We want:

```text
3 → 2 → 1 → 4
```

So the last node of the reversed section must point to `4`.

Therefore:

```javascript
prev = groupNext;
```

Instead of:

```javascript
prev = null;
```

---

# The Standard Reversal Pattern

```javascript
const nextNode = curr.next;
curr.next = prev;
prev = curr;
curr = nextNode;
```

Think of it as:

```text
SAVE → FLIP → MOVE → MOVE
```

### 1. Save the next node

```javascript
const nextNode = curr.next;
```

We must save it before changing `curr.next`, otherwise we lose the rest of the list.

### 2. Reverse the pointer

```javascript
curr.next = prev;
```

### 3. Move `prev`

```javascript
prev = curr;
```

### 4. Move `curr`

```javascript
curr = nextNode;
```

---

# Reversal Example

Suppose:

```text
1 → 2 → 3 → 4
```

and we only want to reverse:

```text
1 → 2 → 3
```

Initial state:

```text
prev = 4
curr = 1
```

### First iteration

```text
nextNode = 2
1.next = 4
```

Now:

```text
prev → 1 → 4
curr → 2 → 3 → 4
```

Move:

```text
prev = 1
curr = 2
```

### Second iteration

```text
nextNode = 3
2.next = 1
```

Now:

```text
prev → 2 → 1 → 4
curr → 3 → 4
```

Move:

```text
prev = 2
curr = 3
```

### Third iteration

```text
nextNode = 4
3.next = 2
```

Now:

```text
3 → 2 → 1 → 4
```

Move:

```text
prev = 3
curr = 4
```

Now:

```text
curr === groupNext
```

Stop.

The group has been reversed.

---

# Connecting the Reversed Group

After reversal, we need to connect it back to the previous part of the list.

```javascript
const oldGroupHead = groupPrev.next;

groupPrev.next = kth;

groupPrev = oldGroupHead;
```

Suppose we started with:

```text
dummy → 1 → 2 → 3
   ↑
groupPrev
```

After reversing `1 → 2`:

```text
2 → 1 → 3
```

### Save the old head

```javascript
const oldGroupHead = groupPrev.next;
```

So:

```text
oldGroupHead = 1
```

The old head becomes the **tail of the reversed group**.

### Connect the previous part to the new head

```javascript
groupPrev.next = kth;
```

Since `kth = 2`:

```text
dummy → 2 → 1 → 3
```

### Move `groupPrev`

```javascript
groupPrev = oldGroupHead;
```

So:

```text
dummy → 2 → 1 → 3 → 4 → 5
            ↑
         groupPrev
```

Now `groupPrev` is exactly where we need it for the next group.

---

# Complete Algorithm

```text
dummy → linked list
        ↓
groupPrev

while there is a complete group of k nodes:

    1. Find kth
    2. groupNext = kth.next
    3. Reverse nodes from groupPrev.next until groupNext
    4. Save oldGroupHead
    5. groupPrev.next = kth
    6. groupPrev = oldGroupHead

return dummy.next
```

---

# Complete JavaScript Solution

```javascript
var reverseKGroup = function(head, k) {
    const dummy = new ListNode(0);
    dummy.next = head;

    let groupPrev = dummy;

    while (true) {
        // Find the kth node
        let kth = groupPrev;

        for (let i = 0; i < k; i++) {
            kth = kth.next;

            if (kth === null) {
                return dummy.next;
            }
        }

        // Node after the current group
        const groupNext = kth.next;

        // Reverse the current group
        let prev = groupNext;
        let curr = groupPrev.next;

        while (curr !== groupNext) {
            const nextNode = curr.next;

            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }

        // Connect the reversed group
        const oldGroupHead = groupPrev.next;

        groupPrev.next = kth;

        // Move to the next group
        groupPrev = oldGroupHead;
    }
};
```

---

# Why Fewer Than `k` Nodes Stay Unchanged

Before reversing a group, we first try to find its `kth` node.

If:

```javascript
kth === null
```

then there aren't enough nodes to form a complete group.

Therefore:

```javascript
return dummy.next;
```

We leave the remaining nodes as they are.

---

# Complexity

## Time Complexity

```text
O(n)
```

Every node is visited a constant number of times.

## Space Complexity

```text
O(1)
```

No extra array or linked list is created.

---

# Mental Model

Think of every iteration as:

```text
FIND
 ↓
[1 → 2 → 3] → rest
 ↓
REVERSE
 ↓
[3 → 2 → 1] → rest
 ↓
CONNECT
 ↓
previous → [3 → 2 → 1] → rest
 ↓
MOVE TO NEXT GROUP
```

The hardest part of this problem is not the reversal itself.

It is understanding that:

```text
groupPrev = node BEFORE the group
kth       = LAST node OF the group
groupNext = node AFTER the group
```

Once those three positions are clear, the rest is standard linked-list pointer manipulation.