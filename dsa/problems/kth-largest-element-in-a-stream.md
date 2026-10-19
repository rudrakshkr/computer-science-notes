# LeetCode 703 — Kth Largest Element in a Stream

## Pattern
Min-Heap of Size `k` + Streaming Data

## Core Idea

Maintain a min-heap containing only the `k` largest values seen so far.

- Insert each new value into the heap.
- If the heap size exceeds `k`, remove the smallest element.
- The root, `heap[0]`, is the kth largest element.

The heap must persist between `add()` calls because numbers arrive over time.

## Why a Min-Heap?

We want to keep the `k` largest values, but we need quick access to the smallest among them.

That smallest value is the **kth largest overall**.

Example:

    k = 3
    Numbers seen = [4, 5, 8, 2, 3]

    Three largest values = [4, 5, 8]

    Min-heap root = 4
    Answer = 4

## Algorithm

### Constructor

1. Store `k`.
2. Initialize an empty persistent heap.
3. Insert each initial number using `add()`.

### add(val)

1. Push `val` into the heap.
2. Call `bubbleUp()` to restore the min-heap property.
3. If `heap.length > k`:
   - Remove the last element using `pop()`.
   - Place it at the root, replacing the minimum.
   - Call `bubbleDown()` to restore the heap.
4. Return `heap[0]`.

## Heap Operations

### Bubble Up

After insertion:

- Compare the new element with its parent.
- Swap while the child is smaller than the parent.
- Continue upward until the min-heap property is restored.

Parent index:

    Math.floor((index - 1) / 2)

### Bubble Down

After replacing the root:

- Find the left and right children.
- Select the smaller child.
- Swap if the parent is greater than that child.
- Continue downward until the min-heap property is restored.

Child indices:

    left  = 2 * index + 1
    right = 2 * index + 2

## Key Invariant

The heap stores at most the `k` largest values encountered so far.

Once it contains `k` values:

    heap[0] = kth largest value

The heap is not sorted. Only the parent-child min-heap property is guaranteed.

## Example

    k = 3
    nums = [4, 5, 8, 2]

    Initial heap retains the 3 largest values:
    [4, 5, 8]

    add(3)  → returns 4
    add(5)  → returns 5
    add(10) → returns 5
    add(9)  → returns 8
    add(4)  → returns 8

Each `add()` updates the existing heap instead of rebuilding it.

## Complexity

Let `n` be the number of initial elements.

- **Constructor:** O(n log k)
- **Each add():** O(log k)
- **Auxiliary space:** O(k)

Each insertion and removal takes O(log k), and the heap never stores more than `k` elements after an operation completes.