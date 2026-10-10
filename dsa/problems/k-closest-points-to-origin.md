# LeetCode 973 — K Closest Points to Origin

## Pattern
**Max-Heap + Top K + Custom Comparison**

## Problem

Given an array of points `points[i] = [x, y]` and an integer `k`, return the `k` points closest to the origin `(0, 0)`.

The answer can be returned in any order.

Example:

    points = [[1,3], [-2,2], [5,8], [0,1]]
    k = 2

    Output = [[0,1], [-2,2]]

## Core Idea

Maintain a **max-heap of size `k`** containing the `k` closest points encountered so far.

Why a max-heap?

- We want the `k` smallest distances.
- When the heap exceeds size `k`, we must remove the farthest point.
- A max-heap keeps the farthest point at the root, allowing efficient removal.

    Max-Heap of size k
          ↓
    Root = farthest retained point

## Distance Formula

The Euclidean distance from `(x, y)` to the origin is:

    distance = √(x² + y²)

We do not need the square root because comparing squared distances gives the same ordering.

    distance² = x² + y²

Helper function:

    const distance = ([x, y]) => x * x + y * y;

This avoids unnecessary square-root calculations.

## Algorithm

1. Create an empty heap.
2. Iterate through every point.
3. Insert the point into the heap.
4. Restore the max-heap using `bubbleUp()`.
5. If the heap size exceeds `k`:
   - Remove the last element and store it.
   - Put it at the root, replacing the previous root.
   - Restore the heap using `bubbleDown()`.
6. Return the remaining points.

## Bubble Up — Max-Heap

After insertion, the new point is at the last index.

For a node at index `i`:

    parentIndex = Math.floor((i - 1) / 2)

Compare squared distances:

    distance(heap[i]) > distance(heap[parentIndex])

If true, swap the child with its parent and continue upward.

Stop when the parent has a greater than or equal squared distance, or the node reaches the root.

## Bubble Down — Max-Heap

After replacing the root, restore the heap property.

For a node at index `i`:

    left  = 2 * i + 1
    right = 2 * i + 2

Steps:

1. Stop if the node has no children.
2. Find the child with the greater squared distance.
3. If the parent has a greater than or equal squared distance, stop.
4. Otherwise, swap the parent with that child.
5. Continue from the child's index.

**Always choose the child with the greater distance** when bubbling down a max-heap.

## Removing the Farthest Point

When the heap size exceeds `k`:

    const last = heap.pop();
    heap[0] = last;
    this.bubbleDown(heap);

This removes the previous root, moves the last element to the root, and restores the max-heap property.

## Example Walkthrough

    points = [[1,3], [-2,2], [5,8], [0,1]]
    k = 2

Squared distances:

    [1,3]   → 10
    [-2,2]  → 8
    [5,8]   → 89
    [0,1]   → 1

Processing the points:

- Insert `[1,3]`: heap contains one point.
- Insert `[-2,2]`: retain both points.
- Insert `[5,8]`: heap size becomes 3, so remove the farthest point, `[5,8]`.
- Insert `[0,1]`: heap size becomes 3, so remove the farthest retained point, `[1,3]`.

Final points:

    [[0,1], [-2,2]]

These are the two closest points to the origin.

## Key Invariant

After each iteration, the heap contains at most `k` points.

Once `k` points have been processed, the heap retains the `k` closest points among all points seen so far.

The root is the farthest among the retained points, making it the point to remove when a closer candidate arrives.

## Complexity

Let `n` be the number of points.

**Time:** `O(n log k)`

Each insertion and possible removal takes `O(log k)` time.

**Auxiliary space:** `O(k)`

The heap stores at most `k` points.

## Common Mistakes

- Using a min-heap instead of a max-heap.
- Comparing point arrays directly instead of comparing their squared distances.
- Choosing the smaller child during max-heap bubble down.
- Forgetting to recalculate child indices after swapping.
- Returning a sorted result unnecessarily; the problem allows any order.

## Recognition

Consider a **max-heap of size `k`** when:

- You need the `k` smallest values.
- You need the `k` closest points.
- You need to discard the largest or worst candidate whenever the collection exceeds `k`.

The opposite pattern is a **min-heap of size `k`** for retaining the `k` largest values, as in LeetCode 215.

## Key Takeaway

**To find the `k` closest points, maintain a max-heap of size `k` using squared distances. Whenever the heap exceeds `k`, remove its root—the farthest retained point.**