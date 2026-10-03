# Heap / Priority Queue

## Pattern

### Core Idea

A **heap** is a complete binary tree commonly represented using an array.

A **min-heap** maintains:

    parent <= children

A **max-heap** maintains:

    parent >= children

The root always contains the extreme value:

    Min-Heap → smallest value
    Max-Heap → largest value

---

## Array Representation

For a node at index `i`:

    parent = Math.floor((i - 1) / 2)
    left   = 2 * i + 1
    right  = 2 * i + 2

No explicit tree nodes are required.

---

## Bubble Up / Sift Up

Used after inserting a new element.

The new element is placed at the end of the heap.

For a min-heap:

    while child < parent:
        swap
        move upward

Pattern:

    insert at end
        ↓
    compare with parent
        ↓
    swap if child is smaller
        ↓
    move to parent's index
        ↓
    repeat

Time:

    O(log n)

---

## Bubble Down / Sift Down

Used after removing the root and replacing it with the last element.

For a min-heap:

1. Start at the root.
2. Find the left and right children.
3. Choose the smaller child.
4. If parent <= smaller child, stop.
5. Otherwise swap.
6. Move down to the smaller child's index.
7. Repeat.

Pattern:

    root replaced
        ↓
    compare with children
        ↓
    choose smaller child
        ↓
    swap if parent is larger
        ↓
    move downward
        ↓
    repeat

Time:

    O(log n)

---

## Removing the Root

For a min-heap, the root is the smallest element.

To remove it efficiently:

    1. Remove the last element from the array.
    2. Put that element at index 0.
    3. Bubble it down.

This avoids shifting the entire array.

---

## Common Heap Operations

    Insert             → O(log n)
    Peek root          → O(1)
    Remove root        → O(log n)

Building a heap from `n` elements can be done in:

    O(n)

---

## When to Recognize a Heap

Think about a heap when the problem involves:

- Finding the smallest or largest element repeatedly.
- Finding the kth smallest/largest element.
- Maintaining the top `k` elements.
- Continuously processing values according to priority.
- Scheduling or processing the highest/lowest priority item.
- Merging sorted collections.

A particularly important pattern:

    "Keep only the k best/largest/smallest elements"

often leads to a heap of size `k`.

---

## Min-Heap vs Max-Heap

Use a **min-heap** when you need quick access to the smallest value.

Use a **max-heap** when you need quick access to the largest value.

For top `k` largest:

    Min-heap of size k

For top `k` smallest:

    Max-heap of size k

---

## Key Insight

A heap is **not a sorted array**.

It only guarantees the parent-child relationship.

For a min-heap:

    [1, 5, 2, 8, 7, 4]

is valid even though the array is not sorted.

The important guarantee is:

    every parent <= its children