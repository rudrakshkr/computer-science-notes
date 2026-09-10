# Lowest Common Ancestor of a Binary Search Tree — LeetCode 235

## Core Idea

Use the **Binary Search Tree property**:

`left < root < right`

At each node, determine where `p` and `q` lie relative to the current node.

## Algorithm

1. If `root === null` → return `null`.
2. If the current node lies between `p` and `q`:
   - `p <= root <= q`
   - or `q <= root <= p`
   → current `root` is the LCA.
3. If both `p` and `q` are smaller than `root`:
   → search the left subtree.
4. If both are larger than `root`:
   → search the right subtree.
5. Return the resulting node.

## Key Insight

If `p` and `q` are on **opposite sides** of the current node, the current node is their lowest common ancestor.

If both are on the same side, the LCA must be deeper on that side.

Because this is a **BST**, we only follow one path instead of searching both subtrees.

## Important Detail

Return the actual **TreeNode**, not its value.

Also, recursive calls must pass `p` and `q` themselves, not `p.val` and `q.val`.

## Complexity

- Time → `O(h)`
- Space → `O(h)` due to recursion stack

`h` = height of the BST.

- Balanced BST → `O(log n)`
- Skewed BST → `O(n)`