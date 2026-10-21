# 295. Find Median from Data Stream

**Pattern:** Two Heaps / Priority Queue  
**Difficulty:** Hard

## 1. Problem Summary

Design a data structure that supports:

- `addNum(num)`: Add a number to the data stream.
- `findMedian()`: Return the median of all numbers added so far.

The median is:
- The middle element when the total number of elements is odd.
- The average of the two middle elements when the total number of elements is even.

The challenge is to find the median efficiently without sorting the entire collection after every insertion.

---

## 2. Core Idea — Two Heaps

Split all numbers into two halves:

1. **Max-heap (`maxHeap`)**: Stores the smaller half of the numbers.
2. **Min-heap (`minHeap`)**: Stores the larger half of the numbers.

The roots provide the two elements around the median.

- `maxHeap[0]` gives the largest element in the lower half.
- `minHeap[0]` gives the smallest element in the upper half.

We don't need the entire collection to be sorted. We only need efficient access to the boundary between the two halves.

---

## 3. Important Invariants

Two conditions must always hold.

### Invariant 1: Correct partition

Every number in the max-heap's lower half must be less than or equal to every number in the min-heap's upper half.

Conceptually:

`maxHeap values <= minHeap values`

This ensures the two roots correctly represent the middle boundary.

### Invariant 2: Balanced sizes

The max-heap must contain either:

- The same number of elements as the min-heap, or
- Exactly one more element than the min-heap.

In other words:

`maxHeap.length === minHeap.length`

or

`maxHeap.length === minHeap.length + 1`

The max-heap gets the extra element when the total count is odd. This makes the median easy to retrieve.

---

## 4. How to Insert a Number

When a new number arrives, decide which half it belongs to.

### Step 1: Choose the heap

- If the max-heap is empty, insert the number into it.
- If `num <= maxHeap[0]`, insert into the max-heap.
- Otherwise, insert into the min-heap.

Restore the destination heap's property using `bubbleUp()`.

### Step 2: Rebalance the sizes

**Case A: Max-heap has more than one extra element**

If:

`maxHeap.length > minHeap.length + 1`

Then:
1. Remove the max-heap's root.
2. Insert that value into the min-heap.
3. Restore the min-heap property with `bubbleUp()`.

**Case B: Min-heap has more elements**

If:

`minHeap.length > maxHeap.length`

Then:
1. Remove the min-heap's root.
2. Insert that value into the max-heap.
3. Restore the max-heap property with `bubbleUp()`.

Only one transfer is necessary after each insertion because the heaps were balanced before the new number arrived.

---

## 5. Removing a Heap's Root

A custom array-based heap stores its root at index `0`, regardless of whether it is a min-heap or a max-heap.

To remove the root:

1. Save the root value.
2. Remove the last array element using `pop()`.
3. If elements remain, place the removed last element at index `0`.
4. Call `bubbleDown()` to restore the heap property.
5. Return the saved root.

Important edge case:

If the heap contained only one element, `pop()` empties it. Do not insert the popped value back at index `0`.

For the max-heap, the root is the largest element. For the min-heap, the root is the smallest.

---

## 6. How to Find the Median

Because `addNum()` maintains the size invariant, only two cases are necessary.

### Case A: Both heaps have equal sizes

The total number of elements is even.

The median is the average of the two roots:

`(maxHeap[0] + minHeap[0]) / 2`

### Case B: Max-heap has one extra element

The total number of elements is odd.

The median is:

`maxHeap[0]`

No sorting, popping, or rebalancing is needed inside `findMedian()`.

---

## 7. Example Walkthrough

Insert the numbers:

`5, 2, 8`

### Insert 5

The max-heap is empty, so insert `5` into it.

- Max-heap: `[5]`
- Min-heap: `[]`

Median: `5`

### Insert 2

Since `2 <= maxHeap[0]`, insert `2` into the max-heap.

The max-heap now has two elements and the min-heap has none, so rebalance by moving the max-heap's root (`5`) to the min-heap.

- Max-heap: `[2]`
- Min-heap: `[5]`

Median:

`(2 + 5) / 2 = 3.5`

### Insert 8

Since `8 > maxHeap[0]`, insert `8` into the min-heap.

- Max-heap: `[2]`
- Min-heap: `[5, 8]`

The min-heap now has more elements than the max-heap. Move its root (`5`) to the max-heap.

Final heaps:

- Max-heap: contains `2, 5`
- Min-heap: contains `8`

Median: `5`

The elements do not need to be fully sorted. Only the heap properties and the two invariants need to be maintained.

---

## 8. Complexity Analysis

Let `n` be the number of elements in the stream.

| Operation | Time Complexity |
|---|---|
| `addNum(num)` | `O(log n)` |
| `findMedian()` | `O(1)` |
| Space | `O(n)` |

### Why?

- Insertion into a heap takes `O(log n)`.
- Rebalancing transfers at most one root, requiring heap operations that take `O(log n)`.
- Finding the median only reads the heap sizes and roots, requiring `O(1)`.
- Both heaps together store every number, requiring `O(n)` space.

---

## Key Takeaway

**Use a max-heap for the lower half and a min-heap for the upper half. Maintain the partition and size invariants so that the median is always available from the roots in `O(1)` time.**