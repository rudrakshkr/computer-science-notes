# 543. Diameter of Binary Tree

## Problem

Find the diameter of a binary tree.

The diameter is the length of the longest path between any two nodes.

Important:
- Diameter is measured in **edges**, not nodes.
- The longest path does not necessarily pass through the root.

Example:

        1
       / \
      2   3
     / \
    4   5

Longest path:

4 → 2 → 1 → 3

Diameter = 3 edges

---

## Key Idea

Use bottom-up DFS.

At every node, we need the heights of its left and right subtrees.

Then:

```text
diameter passing through current node
= leftHeight + rightHeight
```

At the same time, the parent needs the current node's height.

So one recursive helper does two jobs:

1. Returns the subtree height.
2. Updates the maximum diameter found so far.

---

## Recursive Logic

For a null node:

```text
height = 0
```

For a non-null node:

```text
leftHeight = helper(left)
rightHeight = helper(right)

diameter through node = leftHeight + rightHeight

height of node = 1 + max(leftHeight, rightHeight)
```

The helper returns the **height**, not the diameter.

The diameter is stored separately.

---

## JavaScript Pattern

```js
helper(node) {
    if (node === null) {
        return 0;
    }

    let leftHeight = this.helper(node.left);
    let rightHeight = this.helper(node.right);

    // Diameter passing through this node
    this.d = Math.max(this.d, leftHeight + rightHeight);

    // Height returned to parent
    return 1 + Math.max(leftHeight, rightHeight);
}

diameterOfBinaryTree(root) {
    this.d = 0;
    this.helper(root);
    return this.d;
}
```

---

## Why `leftHeight + rightHeight`?

The longest path passing through the current node consists of:

```text
left subtree path → current node → right subtree path
```

The left height gives the number of edges from the current node toward the deepest node on the left, and the right height gives the corresponding distance on the right.

Because the helper returns height as:

```js
1 + max(leftHeight, rightHeight)
```

the sum:

```js
leftHeight + rightHeight
```

is exactly the number of edges in the path through the current node.

---

## Important Distinction

The helper returns:

```text
HEIGHT
```

while the class variable stores:

```text
DIAMETER
```

So:

```js
return 1 + Math.max(leftHeight, rightHeight);
```

is for the parent.

While:

```js
this.d = Math.max(this.d, leftHeight + rightHeight);
```

is for the final answer.

---

## Why Not Calculate Height Separately?

A separate `height()` function can cause repeated traversal of subtrees.

That can make the solution:

```text
O(n²)
```

in the worst case.

By calculating height and diameter in the same DFS, every node is processed once.

Therefore:

```text
Time:  O(n)
Space: O(h)
```

where `h` is the height of the tree because of recursive call stack.