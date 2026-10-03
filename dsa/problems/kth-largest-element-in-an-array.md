# LeetCode 215 — Kth Largest Element in an Array

## Pattern

**Min-Heap of Size `k`**

---

## Problem

Given an integer array `nums` and an integer `k`, return the kth largest element.

Example:

    nums = [3,2,1,5,6,4]
    k = 2

    Output = 5

---

## Core Idea

We do not need to keep every element.

We only need the:

    k largest elements seen so far

Use a **min-heap** to store them.

Why a min-heap?

Because among the `k` largest elements, we need quick access to the **smallest one**.

That smallest element is the:

    kth largest element

---

## Example

For:

    nums = [3,2,1,5,6,4]
    k = 2

Maintain a min-heap containing only the 2 largest values.

Eventually:

    [5,6]

Because this is a min-heap:

    heap[0] = 5

Therefore:

    5 = 2nd largest

---

## Algorithm

For every number:

    1. Insert it into the min-heap.
    2. Restore the heap using bubble up.
    3. If heap size becomes greater than `k`:
           remove the root
           move the last element to the root
           bubble down
    4. After processing everything:
           heap[0] is the answer

---

## Why Remove the Smallest?

Suppose:

    k = 3

We only want the 3 largest elements.

If our heap temporarily contains 4 elements:

    [2, 5, 7, 9]

The smallest one, `2`, cannot belong to the top 3.

So remove it.

The remaining elements are:

    [5, 7, 9]

Thus:

    heap size = k

and the root gives us the smallest among the top `k` elements.

---

## Invariant

The most important invariant is:

    The heap contains exactly the k largest values seen so far.

Because it is a min-heap:

    heap[0] = smallest value among those k values

Therefore:

    heap[0] = kth largest value

---

## Implementation Components

### 1. Bubble Up

After inserting a new number:

    start at the last index
    compare with parent
    swap while child < parent
    move upward

For a node at index `index`:

    parentIndex = Math.floor((index - 1) / 2)

---

### 2. Bubble Down

After removing the root:

    start at index 0
    calculate left and right children
    choose the smaller child
    swap if parent > smaller child
    move downward

Child formulas:

    left  = 2 * index + 1
    right = 2 * index + 2

---

### 3. Remove Minimum

For a min-heap:

    const last = heap.pop();
    heap[0] = last;
    bubbleDown(heap);

The root is removed and the last element takes its place.

---

## Why Not Sort?

Sorting gives:

    O(n log n)

This problem only needs the top `k` elements.

By maintaining a heap of size `k`:

    each insertion/removal = O(log k)

For `n` numbers:

    O(n log k)

---

## Complexity

Time:

    O(n log k)

Space:

    O(k)

This is better than sorting when `k` is significantly smaller than `n`.

---

## Important Detail

The heap does **not** contain the entire array.

It contains only:

    the k largest elements encountered so far

That is what keeps the space at:

    O(k)

---

## Common Mistake

Do not use:

    heap[k - 1]

to get the kth largest.

A heap is not sorted.

Instead, for the min-heap approach:

    heap[0]

is the kth largest because the heap contains exactly the top `k` elements and its root is the smallest among them.

---

## Recognition

When you see:

    "Find kth largest"
    "Find kth smallest"
    "Keep top k"
    "Maintain largest/smallest k elements"

consider a heap.

For kth largest:

    min-heap of size k

For kth smallest:

    max-heap of size k

---

## Key Takeaway

**Maintain a min-heap containing only the `k` largest elements. Whenever its size exceeds `k`, remove the smallest element. At the end, the root of the min-heap is the kth largest element.**

This problem teaches:

    Heap
    +
    Bubble Up
    +
    Bubble Down
    +
    Fixed-size Heap
    +
    Top-K Pattern