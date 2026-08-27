# Merge Two Sorted Lists

## Problem

Given the heads of two **sorted singly linked lists**, merge them into one sorted linked list.

Return the head of the merged list.

Example:

`1 → 2 → 4 → null`

`1 → 3 → 4 → null`

Merged:

`1 → 1 → 2 → 3 → 4 → 4 → null`

---

## Pattern

**Linked List — Two Pointers + Dummy Node**

---

## Key Insight

Use pointers for both lists and compare their current nodes.

At each step:

- Choose the node with the smaller value.
- Attach it to the merged list.
- Move the pointer of the list you selected.
- Move the result pointer forward.

---

## Dummy Node

Use a **dummy node** to simplify building the merged list.

```text
dummy
  ↓
null
```

Maintain another pointer:

`result = dummy`

The merged list is built using:

`result.next`

At the end, return:

`dummy.next`

because the dummy node itself is only a placeholder.

---

## Algorithm

1. Create a dummy node.
2. Set `result` to the dummy node.
3. While both lists are not `null`:
   - Compare `list1.val` and `list2.val`.
   - Attach the smaller node to `result.next`.
   - Move the selected list pointer forward.
   - Move `result` forward.
4. When one list becomes `null`, attach the remaining list directly.
5. Return `dummy.next`.

---

## Why Attach the Remaining List Directly?

Once one list is exhausted, the other list is already sorted.

For example:

`merged → 1 → 2 → 3`

`list1 → null`

`list2 → 4 → 5 → 6`

Simply do:

`result.next = list2`

No additional comparisons are needed.

---

## Example

Initial:

```text
list1 → 1 → 2 → 4
list2 → 1 → 3 → 4
```

Compare:

`1 <= 1`

Take from `list1`.

Then compare the next nodes again and continue until one list is exhausted.

Finally:

`1 → 1 → 2 → 3 → 4 → 4 → null`

---

## Complexity

Time: `O(n + m)`

Space: `O(1)`

where `n` and `m` are the lengths of the two input lists.

---

## Interview Takeaway

> Use two pointers to compare the current nodes of both sorted lists, use a dummy node to simplify list construction, and attach the remaining list once one pointer reaches `null`.