# Linked List Cycle

## Problem

Given the head of a linked list, determine whether the list contains a cycle.

Return:

`true` → cycle exists

`false` → no cycle

---

## Pattern

**Floyd's Cycle Detection — Slow/Fast Pointers**

---

## Key Insight

Use two pointers moving at different speeds:

- `slow` → moves 1 node at a time
- `fast` → moves 2 nodes at a time

If a cycle exists, the fast pointer will eventually **catch the slow pointer**.

If no cycle exists, `fast` will eventually reach `null`.

---

## Algorithm

1. Set both `slow` and `fast` to `head`.
2. While `fast` and `fast.next` are not `null`:
   - Move `slow` one step.
   - Move `fast` two steps.
   - If `slow === fast`, a cycle exists.
3. If `fast` reaches `null`, no cycle exists.

---

## Important Detail

The loop condition must ensure that `fast.next` exists before doing:

`fast = fast.next.next`

Use:

`fast !== null && fast.next !== null`

Also compare **node references**, not node values.

Use:

`slow === fast`

not:

`slow.val === fast.val`

Two different nodes can contain the same value.

---

## Complexity

Time: `O(n)`

Space: `O(1)`

---

## Interview Takeaway

> When you need to detect a cycle in a linked list without extra memory, use two pointers moving at different speeds. If they meet, a cycle exists.