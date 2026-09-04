# LeetCode 160 — Intersection of Two Linked Lists

## Problem

Given two singly linked lists, find the node where they **intersect**.

The intersection is based on **node identity**, not node value.

Example:

```text
A: 4 → 1 → 8 → 4 → 5
B:    5 → 6 → 1
             ↘
              8 → 4 → 5
```

The intersection node is the actual shared `8` node.

Two different nodes can have the same value, so:

```js
nodeA === nodeB
```

must be used rather than:

```js
nodeA.val === nodeB.val
```

---

## Approach 1 — Length Difference

### Core Idea

If the two lists have different lengths, align their starting positions by moving the pointer of the longer list forward by the length difference.

Example:

```text
A: 1 → 2 → 3 → 4 → 5
B:     9 → 8 → 4 → 5
```

Lengths:

```text
A = 5
B = 4
```

Difference:

```text
5 - 4 = 1
```

Move `A` ahead by one node:

```text
A: 2 → 3 → 4 → 5
B: 9 → 8 → 4 → 5
```

Now both pointers have the same number of nodes remaining.

Then move both pointers together and compare their references.

### Steps

1. Find the length of both lists.
2. Find the difference between the lengths.
3. Move the pointer of the longer list forward by that difference.
4. Move both pointers together.
5. When:

```js
currA === currB
```

the intersection node has been found.
6. If both reach `null`, there is no intersection.

### Complexity

- Time: `O(n + m)`
- Extra Space: `O(1)`

---

## Approach 2 — Two-Pointer Switching

A more elegant solution avoids calculating the lengths.

Start two pointers:

```js
let pA = headA;
let pB = headB;
```

When `pA` reaches the end of list A, move it to `headB`.

When `pB` reaches the end of list B, move it to `headA`.

```js
while (pA !== pB) {
    pA = pA === null ? headB : pA.next;
    pB = pB === null ? headA : pB.next;
}
```

Finally:

```js
return pA;
```

### Why It Works

Each pointer traverses **both lists**.

```text
pA: list A → list B

pB: list B → list A
```

Therefore, both pointers travel the same total distance:

```text
pA = lengthA + lengthB
pB = lengthB + lengthA
```

The difference in the original list lengths is automatically cancelled out.

If an intersection exists, the pointers meet at the shared node.

If no intersection exists, both eventually become:

```js
null
```

and the loop ends.

### Example

```text
A: 1 → 2 → 3 → 4 → 5
B:     9 → 8 → 4 → 5
```

After reaching the end:

```text
pA: A → B
pB: B → A
```

Both pointers eventually align at:

```text
4
```

so:

```js
pA === pB
```

### Complexity

- Time: `O(n + m)`
- Extra Space: `O(1)`

---

## Final Code — Two-Pointer Switching

```js
getIntersectionNode(headA, headB) {
    let pA = headA;
    let pB = headB;

    while (pA !== pB) {
        pA = pA === null ? headB : pA.next;
        pB = pB === null ? headA : pB.next;
    }

    return pA;
}
```