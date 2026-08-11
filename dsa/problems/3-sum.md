# 3Sum

## 💡 Problem

Find all unique triplets in an array whose sum is `0`.

---

## 🧠 Approach

1. Sort the array.
2. Fix one element using `i`.
3. Use two pointers:
   - `left = i + 1`
   - `right = end`
4. Calculate the three-element sum.
5. Move pointers based on the sum:
   - Sum < 0 → `left++`
   - Sum > 0 → `right--`
   - Sum === 0 → record triplet, then move both.
6. Skip duplicate values to avoid duplicate triplets.

---

## ⭐ Key Insight

> Fix one number, then reduce the remaining problem to a Two Pointers pair-search.

Sorting makes pointer movement predictable.

---

## ⏱ Complexity

Time: O(n²)

Space: O(1) auxiliary space, excluding the output.

---

## Why Two Pointers?

Sorting the array lets us use the order of the elements to intelligently move the pointers.

Instead of checking every possible triplet in **O(n³)**, we fix one element and use two pointers to find the remaining two in **O(n²)** overall.