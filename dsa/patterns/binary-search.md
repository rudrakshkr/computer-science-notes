# Binary Search

## Core Idea

Binary Search works on a **sorted search space**.

Instead of checking every element one by one, repeatedly **eliminate half of the remaining search space**.

### Key Recognition Question

> Is the search space sorted, and can I determine which half cannot contain the answer?

If yes, Binary Search may be applicable.

---

## Search Space

Maintain two boundaries:

- `left` → first possible index
- `right` → last possible index

They represent the current range that may still contain the target.

---

## Algorithm

1. Set `left` to the first valid index.
2. Set `right` to the last valid index.
3. While `left <= right`:
   - Find the middle index.
   - Compare the middle element with the target.
4. If the middle element equals the target:
   - The target is found.
5. If the target is smaller than the middle element:
   - Discard the right half.
   - Move `right` to `mid - 1`.
6. If the target is larger than the middle element:
   - Discard the left half.
   - Move `left` to `mid + 1`.
7. If `left > right`, the search space is empty and the target does not exist.

---

## Key Insight

The important part of Binary Search is not simply finding the middle.

It is being able to prove that **an entire half of the search space can be discarded**.

Because the array is sorted:

- `target < middle` → search left
- `target > middle` → search right
- `target === middle` → found

---

## Search Insert Position

When the target is not found, the search eventually ends with:

`left > right`

At that point, `left` is the correct index where the target should be inserted to keep the array sorted.

### Key Insight

> `left` represents the first position where the target could be placed.

For example:

`nums = [1, 3, 5, 6]`

`target = 2`

The search ends with:

`left = 1`

So the insertion position is `1`.

---

## Complexity

Time: `O(log n)`

Space: `O(1)`