# LeetCode 105 — Construct Binary Tree from Preorder and Inorder Traversal

## Core Idea

Use the two traversal properties:

- **Preorder:** `Root → Left → Right`
  - The first unused element is always the root of the current subtree.
- **Inorder:** `Left → Root → Right`
  - Once the root is known, its position in inorder tells us exactly which nodes belong to the left and right subtrees.

So for every subtree:

1. Take the next value from `preorder` as the root.
2. Find that root's index in `inorder`.
3. Everything left of that index belongs to the left subtree.
4. Everything right of that index belongs to the right subtree.
5. Recursively build both subtrees.

---

## Why a Preorder Pointer Works

We do not need to create separate preorder arrays for every recursive call.

Maintain:

```text
preIndex = 0
```

Whenever a subtree is created:

```js
const rootVal = preorder[preIndex++];
```

This consumes preorder in exactly the required order:

```text
3 → 9 → 20 → 15 → 7
```

The `inorder` range determines where each subtree ends.

---

## Example

```text
preorder = [3, 9, 20, 15, 7]
inorder  = [9, 3, 15, 20, 7]
```

### Step 1: Root

First preorder value:

```text
3
```

Find `3` in inorder:

```text
[9]  3  [15, 20, 7]
```

Therefore:

```text
left subtree  = [9]
right subtree = [15, 20, 7]
```

### Step 2: Build left subtree

The next preorder value is:

```text
9
```

Its inorder range contains only `9`, so:

```text
    3
   /
  9
```

### Step 3: Build right subtree

Next preorder value:

```text
20
```

Inorder:

```text
[15]  20  [7]
```

So:

```text
    20
   /  \
  15   7
```

Final tree:

```text
      3
     / \
    9   20
       /  \
      15   7
```

---

## Recursive Definition

Let:

```js
helper(left, right)
```

represent:

> Build the subtree using the portion of `inorder` from index `left` to index `right`.

### Base Case

If:

```js
left > right
```

there are no nodes in this subtree:

```js
return null;
```

### Recursive Case

1. Take the next preorder value.
2. Create its node.
3. Find its position in inorder.
4. Build the left subtree.
5. Build the right subtree.
6. Return the root.

```js
root.left = helper(left, mid - 1);
root.right = helper(mid + 1, right);
```

---

## Optimized Approach

Searching for the root's position in `inorder` every time would make the solution `O(n²)` in the worst case.

Use a `Map`:

```text
value → index in inorder
```

Example:

```js
{
  9: 0,
  3: 1,
  15: 2,
  20: 3,
  7: 4
}
```

Then finding `mid` is `O(1)`.

---

## Code

```js
var buildTree = function(preorder, inorder) {
  let preIndex = 0;

  const indexMap = new Map();

  for (let i = 0; i < inorder.length; i++) {
    indexMap.set(inorder[i], i);
  }

  function helper(left, right) {
    if (left > right) {
      return null;
    }

    const rootVal = preorder[preIndex++];
    const root = new TreeNode(rootVal);

    const mid = indexMap.get(rootVal);

    root.left = helper(left, mid - 1);
    root.right = helper(mid + 1, right);

    return root;
  }

  return helper(0, inorder.length - 1);
};
```

---

## Dry Run

```text
preorder = [3, 9, 20, 15, 7]
inorder  = [9, 3, 15, 20, 7]
```

Start:

```text
preIndex = 0
helper(0, 4)
```

### `helper(0, 4)`

```text
rootVal = 3
mid = 1
```

Create:

```text
3
```

Build left:

```text
helper(0, 0)
```

Build right:

```text
helper(2, 4)
```

### `helper(0, 0)`

```text
rootVal = 9
mid = 0
```

Left:

```text
helper(0, -1) → null
```

Right:

```text
helper(1, 0) → null
```

Returns `9`.

### `helper(2, 4)`

```text
rootVal = 20
mid = 3
```

Left:

```text
helper(2, 2)
```

Right:

```text
helper(4, 4)
```

This creates:

```text
   20
  /  \
15    7
```

---

## Complexity

Let `n` be the number of nodes.

### Time

```text
O(n)
```

Each node is processed once, and the `Map` gives `O(1)` inorder lookups.

### Space

```text
O(n)
```

- `Map` → `O(n)`
- Recursion stack → `O(n)` in the worst case

---

## Recognition Pattern

This problem is a classic **tree construction from traversals** problem.

Look for:

```text
Preorder + Inorder
```

The key combination is:

```text
Preorder → tells you WHAT the root is
Inorder  → tells you WHERE the root splits the tree
```