# LeetCode 1046 — Last Stone Weight

## Pattern
Max-Heap / Priority Queue

## Core Idea
Repeatedly remove the two heaviest stones from a max-heap.

- If both stones have equal weight, destroy both.
- Otherwise, insert their difference back into the heap.
- Continue until at most one stone remains.

Return the remaining stone's weight, or `0` if no stones remain.

## Algorithm

1. Insert all stones into a max-heap using `bubbleUp()`.
2. While `heap.length > 1`:
   - Remove the largest stone using `removeMax()`.
   - Remove the second-largest stone.
   - If they differ, insert their difference and bubble up.
3. Return `heap[0]`, or `0` if the heap is empty.

## Custom Max-Heap Operations

**`bubbleUp()`**
- Start at the last index.
- Swap with the parent while the child is larger.
- Continue upward until the max-heap property is restored.

**`removeMax()`**
- Save the root, which is the largest element.
- Remove the last element using `pop()`.
- If elements remain, place the last element at the root.
- Call `bubbleDown()`.
- Return the saved maximum.

**`bubbleDown()`**
- Compare the parent with its children.
- Select the child with the greater value.
- Swap if the child is larger than the parent.
- Continue downward until the heap property is restored.

## Example

    stones = [2, 7, 4, 1, 8, 1]

    Smash 8 and 7 → insert 1
    Smash 4 and 2 → insert 2
    Smash 2 and 1 → insert 1
    Smash 1 and 1 → destroy both

    Remaining stone = 1

## Complexity

For `n` stones:

- **Time:** O(n log n)
- **Space:** O(n)

Building the heap through repeated insertions costs O(n log n). Each smash performs at most two removals and one insertion, each costing O(log n).

## Key Takeaway

**Use a max-heap when you repeatedly need the two largest elements. Remove both efficiently, insert their difference if nonzero, and repeat until at most one element remains.**