# Search in Rotated Sorted Array

## Problem

Given a **rotated sorted array** and a target value, return the index of the target.

If the target does not exist, return `-1`.

---

## Pattern

**Modified Binary Search**

---

## Key Insight

Although the entire array is not sorted after rotation, **at least one half of the current search space is always sorted**.

At each iteration:

1. Find `mid`.
2. If `nums[mid] === target`, return `mid`.
3. Determine which half is sorted.
4. Check whether the target lies inside that sorted half.
5. Search the appropriate half.

---

## Identify the Sorted Half

### Left Half Is Sorted

If:

`nums[left] <= nums[mid]`

then the left portion is sorted.

Check whether the target lies inside it:

`nums[left] <= target < nums[mid]`

If true:

`right = mid - 1`

Otherwise:

`left = mid + 1`

---

### Right Half Is Sorted

Otherwise, the right portion is sorted.

Check whether the target lies inside it:

`nums[mid] < target <= nums[right]`

If true:

`left = mid + 1`

Otherwise:

`right = mid - 1`

---

## Algorithm

1. Set `left = 0`.
2. Set `right = nums.length - 1`.
3. While `left <= right`:
   - Calculate `mid`.
   - Check whether `nums[mid]` is the target.
   - Determine which half is sorted.
   - Check whether the target belongs to that sorted half.
   - Discard the other half.
4. Return `-1` if the target is not found.

---

## Important Detail

Always check:

`nums[mid] === target`

before deciding which half to search.

The search space uses inclusive boundaries:

`left <= right`

---

## Example

`nums = [4, 5, 6, 7, 0, 1, 2]`

`target = 0`

The left half is initially sorted:

`[4, 5, 6, 7]`

Since `0` does not belong to that sorted range, discard it and search the right half.

Eventually the target is found at index `4`.

---

## Complexity

Time: `O(log n)`

Space: `O(1)`

---

## Interview Takeaway

> In a rotated sorted array, identify the sorted half first, then determine whether the target lies inside that half. This allows Binary Search to discard half of the search space at every iteration.