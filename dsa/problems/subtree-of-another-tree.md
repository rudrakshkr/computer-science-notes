# Subtree of Another Tree — LeetCode 572

## Core Idea

A tree `subRoot` is a subtree of `root` if there is some node in `root` where the entire tree starting from that node is **identical** to `subRoot`.

This combines two recursive tasks:

1. **Traverse `root`** to check every possible starting node.
2. **Compare two trees** using the Same Tree logic.

## Algorithm

1. If `root === null` → return `false`.
2. Check whether the tree rooted at `root` is the same as `subRoot`.
   - If yes → return `true`.
3. Otherwise, recursively search:
   - left subtree of `root`
   - right subtree of `root`
4. Return `true` if either side contains `subRoot`.

Conceptually:

```text
isSubtree(root, subRoot)

    if root is null
        return false

    if isSameTree(root, subRoot)
        return true

    return
        isSubtree(root.left, subRoot)
        OR
        isSubtree(root.right, subRoot)
```

## Key Insight

The problem is essentially:

```text
Find a possible starting node
        +
Check if the two trees are identical
```

The `isSameTree()` helper checks:

- Both nodes are `null` → `true`
- One is `null` → `false`
- Values differ → `false`
- Otherwise compare both left and right subtrees

## Why `OR`?

`subRoot` only needs to exist in **one** side of `root`.

So:

`left subtree contains it OR right subtree contains it`

Using `AND` would incorrectly require the same subtree to exist in both sides.

## Complexity

Let:

- `m` = number of nodes in `root`
- `n` = number of nodes in `subRoot`

### Time

`O(m × n)` in the worst case.

We may compare `subRoot` against many nodes in `root`, and each comparison can take up to `O(n)`.

### Space

`O(h₁ + h₂)` auxiliary space for the recursion stacks, where:

- `h₁` = height of `root`
- `h₂` = height of `subRoot`

Worst case for skewed trees:

`O(m + n)`