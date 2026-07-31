# Bucket Sort Pattern

---

## When to Use

Use Bucket Sort when:

- Values are within a **small known range**
- Sorting is unnecessary or too expensive
- You want better than **O(n log n)**

---

## Core Idea

Instead of sorting values directly,

store them in buckets where the **index represents the value** (or frequency).

Example:

```text
Frequency

1 -> [3]
2 -> [2]
3 -> [1, 4]
```

Traverse buckets in the required order.

---

## Common Steps

1. Determine the range.
2. Create buckets.
3. Place elements into buckets.
4. Traverse buckets.

---

## Complexity

### Time

O(n)

### Space

O(n)

---

## Common Problems

- Top K Frequent Elements
- Sort Characters By Frequency
- Bucket-based Frequency Problems

---

## Recognition Clues

Think about Bucket Sort if:

- The problem involves frequencies.
- The values are bounded (e.g. `1...n`).
- The follow-up asks for **better than O(n log n)**.
- You only need the highest/lowest values instead of a fully sorted order.