# Maximum Depth of Binary Tree — LeetCode 104

## Problem

Find the **maximum depth** (number of nodes along the longest path from the root to a leaf) of a binary tree.

## Core Idea

For any node:

`depth(node) = 1 + max(depth(left), depth(right))`

The current node contributes `1`, and we take the deeper of its two subtrees.

## Algorithm

1. If the current node is `null`, return `0`.
2. Recursively find the maximum depth of the left subtree.
3. Recursively find the maximum depth of the right subtree.
4. Return:

`1 + max(leftDepth, rightDepth)`

The recursive calls return the depth of each subtree to its parent.

## Example

```text
        1
       / \
      2   3
     /
    4
```

Depths returned upward:

`depth(4) = 1`

`depth(2) = 2`

`depth(3) = 1`

`depth(1) = 3`

Answer:

`3`

## Key Insight

Do not maintain a separate counter while traversing.

Instead:

> **Each recursive call returns the maximum depth of its subtree.**

The parent uses those returned values to calculate its own depth.

## Complexity

- Time → `O(n)` because every node is visited once.
- Space → `O(h)` because of the recursion stack.

Where `h` is the height of the tree.

- Balanced tree → `O(log n)`
- Skewed tree → `O(n)`