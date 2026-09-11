# Balanced Binary Tree — LeetCode 110

## Core Idea

A binary tree is balanced if, for every node, the height difference between its left and right subtrees is at most `1`.

Instead of separately calculating:

- whether the tree is balanced
- the height of each subtree

use **one recursive helper** to do both.

## `-1` Technique

The helper returns:

- **height** → subtree is balanced
- **`-1`** → subtree is unbalanced

`-1` acts as a signal that the imbalance has already been detected.

## Algorithm

1. If the node is `null` → return `0`.
2. Recursively get the left subtree's result.
3. Recursively get the right subtree's result.
4. If either result is `-1` → return `-1`.
5. If the height difference is greater than `1` → return `-1`.
6. Otherwise return the subtree's height:

`1 + max(leftHeight, rightHeight)`

Finally:

`isBalanced(root) → helper(root) !== -1`

## Key Insight

A single recursive function carries two types of information:

```text
positive value → balanced + subtree height
-1             → subtree is unbalanced
```

Once `-1` appears, every parent can immediately propagate it upward without calculating unnecessary heights.

This avoids repeatedly traversing subtrees with a separate `maxDepth()` function.

## Complexity

- Time → `O(n)` because every node is visited once.
- Space → `O(h)` due to the recursion stack.

`h` = height of the tree.

- Balanced tree → `O(log n)`
- Skewed tree → `O(n)`