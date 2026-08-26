# Reverse Linked List

## Problem

Given the head of a singly linked list, reverse the list and return the new head.

Example:

`1 → 2 → 3 → 4 → 5 → null`

becomes:

`5 → 4 → 3 → 2 → 1 → null`

---

## Pattern

**Linked List — Pointer Manipulation**

---

## Key Insight

To reverse the list, reverse each node's `next` pointer.

Before changing `current.next`, **save the next node** so the rest of the list is not lost.

---

## Three Pointers

Maintain:

- `prev` → previous node
- `current` → current node
- `next` → saved next node

Initial state:

`prev = null`

`current = head`

---

## Algorithm

For every node:

1. Save the next node:

   `next = current.next`

2. Reverse the current pointer:

   `current.next = prev`

3. Move `prev` forward:

   `prev = current`

4. Move `current` forward:

   `current = next`

5. Continue until `current === null`.

At the end:

> `prev` is the new head of the reversed list.

---

## Example

Starting with:

`1 → 2 → 3 → null`

After reversing `1`:

`null ← 1    2 → 3`

After reversing `2`:

`null ← 1 ← 2    3`

After reversing `3`:

`null ← 1 ← 2 ← 3`

So:

`prev → 3 → 2 → 1 → null`

Return `prev`.


---

## Complexity

Time: `O(n)`

Space: `O(1)`