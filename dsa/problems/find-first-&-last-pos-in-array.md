# Find First and Last Position of Element in Sorted Array

## Problem

Given a **sorted array** and a target value, return the starting and ending position of the target.

If the target does not exist, return `[-1, -1]`.

---

## Pattern

**Binary Search**

---

## Key Insight

A normal Binary Search can find **an occurrence** of the target.

To find the **first and last occurrence**, perform **two separate Binary Searches**:

- First search → keep moving left after finding the target.
- Second search → keep moving right after finding the target.

---

## First Occurrence

When:

`nums[mid] === target`

Save the index:

`result[0] = mid`

But do **not** stop.

Continue searching left:

`right = mid - 1`

This allows us to find an earlier occurrence.

---

## Last Occurrence

When:

`nums[mid] === target`

Save the index:

`result[1] = mid`

But do **not** stop.

Continue searching right:

`left = mid + 1`

This allows us to find a later occurrence.

---

## Algorithm

### First Binary Search

1. Perform normal Binary Search.
2. When the target is found, store `mid` as the first occurrence.
3. Continue searching the left half.
4. Keep updating the stored index whenever another occurrence is found.

### Second Binary Search

1. Reset `left` and `right`.
2. Perform another Binary Search.
3. When the target is found, store `mid` as the last occurrence.
4. Continue searching the right half.
5. Keep updating the stored index whenever another occurrence is found.

If the target is never found, return:

`[-1, -1]`

---

## Example

`nums = [5, 7, 7, 8, 8, 10]`

`target = 8`

First occurrence:

`3`

Last occurrence:

`4`

Result:

`[3, 4]`

---

## Complexity

Time: `O(log n)`

Space: `O(1)`

---

## Interview Takeaway

> When Binary Search finds the target but the problem asks for a boundary, **record the current index and continue searching toward that boundary instead of returning immediately**.