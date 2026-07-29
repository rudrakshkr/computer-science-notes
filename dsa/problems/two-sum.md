# Two Sum

Difficulty: Easy

---

## Pattern

Hash Map

---

## Intuition

Need fast lookup while iterating once.

---

## Algorithm
1. Iterate through the `nums` array with index `i` once
2. Compute complement = `target - nums[i]` 
3. If difference in HashMap, return the indices
4. Else store the current element in the hashmap with its index and continue

## Complexity

Time: O(n)

Space: O(n)

## Reflection

My first instinct was to search the array for the complement using indexOf(). That works, but it results in O(n²) time because each lookup scans the array. Using a Hash Map trades O(n) extra space for O(n) time by allowing constant-time lookups.