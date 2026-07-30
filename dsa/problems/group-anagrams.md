# Group Anagrams

Difficulty: Medium

---

## Pattern

Hash Map (Grouping)

---

## Intuition

Create a unique signature for every word.
Words with the same signature belong to the same group.

---

## Algorithm

1. Iterate through each word.
2. Build a frequency array of size 26.
3. Convert it into a string signature by incrementing the frequency of each character of word in the array.
4. Use the signature as the key in a Hash Map.
5. Append the word to its corresponding group.
6. Return all grouped values.

---

## Complexity

Time: O(n × k), where n is the number of words and k is the average word length

Space: O(n × k)

---

## Takeaway

A Hash Map can also be used for grouping by mapping a unique signature to a collection of related items.