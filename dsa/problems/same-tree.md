# Same Tree — LeetCode 100

## Core Idea

Two binary trees are the same when they have:

- The same structure
- The same value at every corresponding node

## Algorithm

1. If both nodes are `null` → return `true`.
2. If only one node is `null` → return `false`.
3. If their values are different → return `false`.
4. Recursively compare:
   - left subtree of `p` with left subtree of `q`
   - right subtree of `p` with right subtree of `q`
5. Both subtree comparisons must be `true`.

## Key Insight

Compare the two trees **at corresponding positions**.

At every pair of nodes, first check:

```text
Both null?
One null?
Values different?
```

If none of these conditions occur, continue recursively to both sides.

The final condition is:

`same(left subtrees) && same(right subtrees)`

## Complexity

- Time → `O(n)` in the worst case
- Space → `O(h)` due to recursion stack

`h` = height of the tree.