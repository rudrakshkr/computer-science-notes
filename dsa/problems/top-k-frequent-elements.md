# Top K Frequent Elements

**Difficulty:** Medium

---

## Pattern

- Hash Map
- Bucket Sort

---

## Intuition

1. Count the frequency of every number using a Hash Map.
2. Create buckets where the **index represents the frequency**.
3. Place each number into its corresponding bucket.
4. Traverse buckets from highest frequency to lowest.
5. Stop after collecting `k` elements.

---

## Why Bucket Sort?

Sorting all unique elements takes:

O(n log n)

Since frequencies are limited to **1...n**, we can use the frequency itself as an index.

This avoids sorting completely.

---

## Algorithm

1. Build frequency map.
2. Create `n + 1` buckets.
3. Place each number into `bucket[frequency]`.
4. Traverse buckets backwards.
5. Return the first `k` elements.

---

## Complexity

### Time

O(n)

### Space

O(n)

---

## Takeaways

- Count frequencies using a Hash Map.
- When values are within a small known range, consider Bucket Sort.
- Buckets eliminate the need for sorting.