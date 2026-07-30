# Valid Anagram

Difficulty: Easy

---

## Pattern

Hash Map (Frequency Counting)

---

## Intuition

Need to compare the frequency of each character in both strings.

---

## Algorithm

1. If lengths differ, return `false`.
2. Count the frequency of each character in the first string.
3. Traverse the second string and decrement the frequency.
4. If a character doesn't exist or its frequency becomes negative, return `false`.
5. Return `true`.

---

## Complexity

Time: O(n)

Space: O(n)

---

## Takeaway

Use a Hash Map when you need to keep track of the **frequency** of elements rather than just whether they exist.