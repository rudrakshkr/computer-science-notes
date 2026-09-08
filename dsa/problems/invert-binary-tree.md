# Invert Binary Tree — LeetCode 226

## Core Idea

Invert the tree by swapping the `left` and `right` children of **every node**.

## Algorithm

1. If the current node is `null`, return.
2. Swap its `left` and `right` pointers.
3. Recursively invert the new left subtree.
4. Recursively invert the new right subtree.
5. Return the current node.

## Important Recursion Detail

After swapping the children, recursion follows the **updated pointers**.

The node itself is not swapped again when recursion backtracks; the recursive call simply continues with the next instruction of the parent call.

## Complexity

- Time → `O(n)` because every node is visited once.
- Space → `O(h)` due to recursion stack.

`h` = height of the tree.