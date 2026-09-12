# Validate Binary Search Tree — LeetCode 98

## Core Idea

A Binary Search Tree must satisfy:

- Every value in the left subtree is **less than** the current node.
- Every value in the right subtree is **greater than** the current node.

Checking only the immediate children is not enough. A node must satisfy the restrictions imposed by **all of its ancestors**.

## Range Technique

Pass a valid range `(left, right)` down through the recursion.

Initially:

`(-∞, +∞)`

For each node:

- Left subtree → `(left, node.val)`
- Right subtree → `(node.val, right)`

The node is valid only when:

`left < node.val < right`

## Algorithm

1. Start the root with range `(-∞, +∞)`.
2. If the node is `null` → return `true`.
3. If `node.val` is outside its allowed range → return `false`.
4. Recursively validate the left subtree with:
   - same lower bound
   - current node as upper bound
5. Recursively validate the right subtree with:
   - current node as lower bound
   - same upper bound
6. Both recursive checks must be valid.

## Example

```text
        5
       / \
      3   7
     / \
    2   6
```

For node `3`:

`(-∞, 5)`

For node `6`:

`(3, 5)`

But:

`3 < 6 < 5` → false

So the tree is not a valid BST.

## Key Insight

The important idea is:

> **A node must satisfy the constraints inherited from every ancestor, not just its parent.**

The valid range becomes narrower as we move down the tree.

## Complexity

- Time → `O(n)` because every node is visited once.
- Space → `O(h)` due to the recursion stack.

`h` = height of the tree.

- Balanced tree → `O(log n)`
- Skewed tree → `O(n)`