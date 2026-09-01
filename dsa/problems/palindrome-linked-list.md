# Palindrome Linked List

## Problem

Given the head of a singly linked list, determine whether the list is a **palindrome**.

Example:

`1 → 2 → 2 → 1 → null`

Result:

`true`

Example:

`1 → 2 → 3 → null`

Result:

`false`

---

## Pattern

**Linked List — Slow/Fast Pointers + In-Place Reversal**

---

## Key Insight

A linked list only allows easy traversal in the forward direction.

To compare the first half with the second half:

1. Find the middle using slow/fast pointers.
2. Reverse the second half.
3. Compare the first half with the reversed second half.
4. Optionally reverse the second half again to restore the original list.

---

## Algorithm

### 1. Find the Middle

Use:

- `slow` → moves 1 node
- `fast` → moves 2 nodes

When `fast` reaches the end:

`slow` is at the middle / start of the second half.

---

### 2. Reverse the Second Half

Start reversing from `slow`.

Maintain:

- `curr`
- `prev`
- `next`

After reversal:

`prev` becomes the head of the reversed second half.

---

### 3. Compare Both Halves

Set:

`left = head`

`right = prev`

Compare:

`left.val === right.val`

Move both pointers forward until `right === null`.

If any values differ:

`false`

If all values match:

`true`

---

## Odd-Length Lists

For a list such as:

`1 → 2 → 3 → 2 → 1`

the middle element does not need to be compared with itself.

Only the corresponding values from the two halves need to match.

---

## Optional: Restore the List

Reversing the second half modifies the original linked list.

To leave the input unchanged:

`reverse → compare → reverse again`

Reverse the second half a second time after comparison.

---

## Complexity

Time: `O(n)`

Space: `O(1)`

---

## Interview Takeaway

> Use slow/fast pointers to find the middle, reverse the second half in place, then compare it with the first half. Reverse the second half again if the original list must be preserved.