# 236. Lowest Common Ancestor of a Binary Tree

## Problem

Given a **normal binary tree** and two nodes `p` and `q`, find their **Lowest Common Ancestor (LCA)**.

Unlike a BST, there is **no ordering relationship** between parent and child values.

Example:

        5
       / \
      3   4
     / \
    2   1

For:

```text
p = 1
q = 2
```

The LCA is:

```text
3
```

---

## Pattern

**Bottom-Up DFS / Recursive Aggregation**

Each recursive call searches its subtree and reports back what it found.

The helper can return:

```text
null → neither target found
p/q  → one target found
node → current node is the LCA
```

---

## Base Cases

### Null node

If the subtree is empty:

```js
if (node === null) {
    return null;
}
```

### Target node

If the current node is either `p` or `q`:

```js
if (node === p || node === q) {
    return node;
}
```

Once a target is found, return it upward.

---

## Core Logic

Search both subtrees:

```js
let left = helper(node.left, p, q);
let right = helper(node.right, p, q);
```

Then interpret what they returned.

### Both sides found something

```text
left !== null
right !== null
```

One target was found in the left subtree and the other in the right subtree.

Therefore:

```js
return node;
```

The current node is the LCA.

### Only left found something

```js
if (left !== null) {
    return left;
}
```

The LCA has not necessarily been found yet, so pass the result upward.

### Only right found something

```js
if (right !== null) {
    return right;
}
```

Again, pass the result upward.

### Neither side found anything

```js
return null;
```

---

## Code

```js
var lowestCommonAncestor = function(root, p, q) {

    function helper(node, p, q) {
        if (node === null) {
            return null;
        }

        if (node === p || node === q) {
            return node;
        }

        let left = helper(node.left, p, q);
        let right = helper(node.right, p, q);

        if (left !== null && right !== null) {
            return node;
        }

        if (left !== null) {
            return left;
        }

        if (right !== null) {
            return right;
        }

        return null;
    }

    return helper(root, p, q);
};
```

---

## Dry Run

Tree:

        5
       / \
      3   4
     / \
    2   1

```text
p = 1
q = 2
```

At `3`:

```text
helper(2) → 2
helper(1) → 1
```

So:

```text
left  = 2
right = 1
```

Both are non-null.

Therefore:

```text
3 is the LCA
```

and:

```js
return 3;
```

That result propagates upward through node `5`.

Final answer:

```text
3
```

---

## Why This Works

The recursion works **bottom-up**.

Instead of asking:

> "Where is the LCA?"

each subtree answers:

> "Did I find `p`, `q`, or the LCA?"

The parent then combines those answers.

The critical condition is:

```js
left !== null && right !== null
```

which means both targets were found on opposite sides of the current node.

---

## Difference from LCA of BST

### BST

Uses ordering:

```text
p < root
q < root
→ go left

p > root
q > root
→ go right
```

### Normal Binary Tree

There is no ordering guarantee.

Therefore:

```text
Search both subtrees
→ collect results
→ determine LCA while backtracking
```

---

## Complexity

```text
Time:  O(n)
Space: O(h)
```

`n` = number of nodes.

`h` = height of the tree due to recursive call stack.

---

Key mental model:

> **Each subtree reports its result upward. The first node that receives a non-null result from both sides is the LCA.**