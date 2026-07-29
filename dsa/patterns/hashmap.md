# Hash Map / Hash Set Pattern

## Core Idea

Use a hash-based data structure when you need **fast O(1) average lookup** while processing data.

The key question to ask yourself is:

> **What information do I need to remember as I process the input?**

---

## When to Use a Hash Set

Use a **Set** when you only care about **whether something has been seen before**.

Examples:

- Detect duplicates
- Check membership
- Ensure uniqueness
- Keep track of visited nodes

### Common Problems

- Contains Duplicate
- Happy Number
- Longest Consecutive Sequence
- Valid Sudoku

---

## When to Use a Hash Map

Use a **Map** when you need to associate **additional information** with a key.

Examples:

- Store value → index
- Store frequency/count
- Store value → object
- Store previous occurrences

### Common Problems

- Two Sum
- Group Anagrams
- Isomorphic Strings
- Top K Frequent Elements
- LRU Cache

---

## Decision Guide

Ask yourself:

### Do I only need to know if I've seen this before?

→ Use a **Set**

Example:

```text
Have I seen the number 5 before?
```

---

### Do I need extra information about what I've seen?

→ Use a **Map**

Examples:

```text
Where did I see this number?

How many times have I seen it?

Which word belongs to this group?
```

---

## Time Complexity (Average Case)

| Operation | Hash Map | Hash Set |
|-----------|----------|----------|
| Lookup | O(1) | O(1) |
| Insert | O(1) | O(1) |
| Delete | O(1) | O(1) |

Worst case: O(n), though this is uncommon with good hash functions.

---

## Common Tradeoff

Hash-based structures usually trade **memory for speed**.

Example:

Array Search

Time: O(n²)

Space: O(1)

↓

Hash Map

Time: O(n)

Space: O(n)

---

## Red Flags

If you notice yourself:

- Using nested loops
- Repeatedly searching an array
- Calling `indexOf()` inside a loop
- Calling `includes()` inside a loop

Ask yourself:

> **Can a Hash Map or Hash Set reduce this to O(n)?**

---

## Interview Habit

Before choosing a data structure, ask:

> **What state do I need to remember while processing the input?**