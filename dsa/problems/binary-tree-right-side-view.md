# 199. Binary Tree Right Side View

## Problem

Given the root of a binary tree, return the values of the nodes that are visible when viewing the tree from the right side.

Example:

        1
       / \
      2   3
       \   \
        5   4

Right-side view:

```text
[1, 3, 4]
```

---

## Pattern

**BFS / Level-Order Traversal**

The tree is processed one level at a time.

At each level, the **last node** is the node visible from the right side.

---

## Key Insight

Using BFS:

```text
Level 1 → [1]       → take 1
Level 2 → [2, 3]    → take 3
Level 3 → [5, 4]    → take 4
```

Therefore:

```text
answer = last node of every level
```

---

## Approach

1. If `root` is null, return an empty array.
2. Put the root into a queue.
3. Use a `front` index instead of `shift()` so queue removal is O(1).
4. Before processing a level, calculate its size:
   ```js
   let queueLength = queue.length - front;
   ```
5. Process exactly `queueLength` nodes.
6. When processing the last node of the level:
   ```js
   if (i === queueLength - 1)
   ```
   add its value to the result.
7. Add the current node's left and right children to the queue.
8. Return the result.

---

## Code

```js
var rightSideView = function(root) {
    let res = [];

    if (!root) return res;

    let queue = [root];
    let front = 0;

    while (front < queue.length) {
        let queueLength = queue.length - front;

        for (let i = 0; i < queueLength; i++) {
            let node = queue[front++];

            // Last node of the current level
            if (i === queueLength - 1) {
                res.push(node.val);
            }

            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
    }

    return res;
};
```

---

## Important Detail

`queueLength` must be calculated **before** adding children:

```js
let queueLength = queue.length - front;
```

This represents only the nodes belonging to the **current level**.

The children added during the loop belong to the **next level**.

---

## Complexity

```text
Time:  O(n)
Space: O(n)
```

Every node is processed once.

The queue can contain up to O(n) nodes in the worst case.

---

## Interview Takeaway

When a problem asks for something related to **each level of a binary tree**, think:

```text
BFS
 ↓
process one level at a time
 ↓
select the required node(s) from that level
```

For the Right Side View:

```text
take the LAST node of each level
```