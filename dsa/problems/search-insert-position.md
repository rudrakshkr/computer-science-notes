# Search Insert Position

## Problem

Given a **sorted array** and a target value, return the index where the target is found.

If the target does not exist, return the index where it should be inserted to keep the array sorted.

---

## Pattern

**Binary Search**

---

## Key Insight

If the target is not found, Binary Search ends when:

`left > right`

At that point:

> `left` is the correct insertion position.

This works because everything before `left` is smaller than the target, while everything from `left` onward is greater than the target.

---

## Approach

1. Perform normal Binary Search.
2. If `nums[mid] === target`, return `mid`.
3. If `nums[mid] < target`, search the right half.
4. If `nums[mid] > target`, search the left half.
5. If the target is not found, return `left`.

---

## Example

`nums = [1, 3, 5, 6]`

`target = 2`

The search ends with:

`left = 1`

So the target should be inserted at index `1`.

---

## Complexity

Time: `O(log n)`

Space: `O(1)`