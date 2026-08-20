# Binary Search — Binary Search

## Problem

Given a **sorted array** and a target value, return the index of the target.

If the target does not exist, return `-1`.

---

## Pattern

**Binary Search**

---

## Example

`nums = [-1, 0, 3, 5, 9, 12]`

`target = 9`

Expected result:

`4`

---

## Approach

1. Start with the entire array as the search space.
2. Find the middle element.
3. If it equals the target, return its index.
4. If the middle element is greater than the target:
   - Search the left half.
5. If the middle element is smaller than the target:
   - Search the right half.
6. Continue until the target is found or the search space becomes empty.
7. Return `-1` if the target is not found.

---

## Important Boundary Detail

The last valid array index is:

`nums.length - 1`

not:

`nums.length`

The search uses an inclusive range, so the loop condition is:

`left <= right`

---

## Complexity

Time: `O(log n)`

Space: `O(1)`

---

## Interview Takeaway

> Use Binary Search when the search space is sorted and each comparison allows you to eliminate roughly half of the remaining possibilities.