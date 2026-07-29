# Contains Duplicate

Difficulty: Easy

---

## Pattern

Hash Set

---

## Intuition

Need to quickly check whether an element has already been seen.

---

## Algorithm

1. Create an empty Hash Set.
2. Iterate through the `nums` array.
3. If the current element already exists in the set, return `true`.
4. Otherwise, add it to the set.
5. If the loop finishes, return `false`.

---

## Complexity

Time: O(n)

Space: O(n)

---

## Why Hash Set?

A **Hash Set** is enough because we only need to know **whether an element has been seen before**.

If we needed to store extra information (like an index or frequency), we would use a **Hash Map** instead.