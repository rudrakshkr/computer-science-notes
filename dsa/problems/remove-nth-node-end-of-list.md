# Remove Nth Node From End of List

## Problem

Given the head of a linked list and an integer `n`, remove the **nth node from the end** of the list.

Return the head of the modified list.

Example:

`1 → 2 → 3 → 4 → 5 → null`

`n = 2`

Result:

`1 → 2 → 3 → 5 → null`

---

## Pattern

**Linked List — Two Pointers + Dummy Node**

---

## Key Insight

Use two pointers with a fixed gap of `n + 1` nodes between them.

This allows `slow` to stop **immediately before the node that needs to be removed** when `fast` reaches `null`.

---

## Algorithm

1. Create a dummy node before `head`.
2. Set both `slow` and `fast` to the dummy node.
3. Move `fast` ahead by `n + 1` nodes.
4. Move `slow` and `fast` together until `fast === null`.
5. `slow.next` is now the node to remove.
6. Skip that node:

`slow.next = slow.next.next`

7. Return `dummy.next`.

---

## Why `n + 1`?

We need `slow` to end **one node before** the target node.

Using a dummy node and a gap of `n + 1` creates exactly that positioning.

---

## Why Use a Dummy Node?

The dummy node makes edge cases simpler, especially when the node being removed is the **head**.

Conceptually:

`dummy → 1 → 2 → 3 → null`

Then the same pointer logic works regardless of which node is removed.

---

## Complexity

Time: `O(n)`

Space: `O(1)`

---

## Interview Takeaway

> Use a dummy node and maintain an `n + 1` node gap between two pointers. When the fast pointer reaches the end, the slow pointer is positioned immediately before the node that must be removed.