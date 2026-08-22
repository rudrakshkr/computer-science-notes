# Search a 2D Matrix

## Problem

Given an `m × n` matrix where:

- Each row is sorted in ascending order.
- The first element of each row is greater than the last element of the previous row.

Return `true` if the target exists, otherwise return `false`.

---

## Pattern

**Binary Search**

---

## Key Insight

Treat the entire matrix as a **virtual sorted 1D array** without actually flattening it.

For a matrix with:

`m × n`

elements, the virtual search space contains:

`m × n` positions.

---

## Mapping 1D Index to 2D

Given a virtual index `mid` and `n` columns:

- `row = Math.floor(mid / n)`
- `col = mid % n`

This converts the virtual 1D position into the corresponding matrix position.

Example:

`mid = 6`

`n = 4`

Then:

- `row = 1`
- `col = 2`

So the element is:

`matrix[1][2]`

---

## Approach

1. Set `left = 0`.
2. Set `right = (rows × cols) - 1`.
3. Find the middle virtual index.
4. Convert `mid` into a row and column.
5. Compare the matrix value with the target.
6. If the value is smaller, search the right half.
7. If the value is larger, search the left half.
8. If the value matches the target, return `true`.
9. If the search space becomes empty, return `false`.

---

## Complexity

Time: `O(log(m × n))`

Space: `O(1)`

---

## Interview Takeaway

> When a 2D matrix has a global sorted order, you can treat it as a virtual 1D sorted array and apply Binary Search without creating an additional array.