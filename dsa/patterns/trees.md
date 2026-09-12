# Trees — Traversal Patterns

## Core Idea

A **Tree** is a hierarchical data structure made of nodes connected by edges.

A **Binary Tree** is a tree where each node has at most two children:

- `left`
- `right`

Example:

```text
        1
       / \
      2   3
     / \
    4   5
```

Each node typically contains:

- `value`
- `left` → reference to the left child
- `right` → reference to the right child

---

## Tree Terminology

- **Root** → topmost node of the tree
- **Parent** → node directly above another node
- **Child** → node directly below another node
- **Leaf** → node with no children
- **Depth** → distance from the root to a node
- **Height** → longest distance from a node to a leaf

---

# DFS — Depth-First Search

DFS explores **as deeply as possible along a path before backtracking**.

For binary trees, DFS is commonly implemented using **recursion** or an explicit **stack**.

There are three common DFS traversals.

## Preorder

`Root → Left → Right`

Process the current node **before** its children.

Example:

```text
        1
       / \
      2   3
     / \
    4   5
```

Traversal:

`1 → 2 → 4 → 5 → 3`

### Recognition

Use when the problem requires processing a node **before** processing its subtrees.

---

## Inorder

`Left → Root → Right`

Process the current node **between** its left and right subtrees.

Traversal:

`4 → 2 → 5 → 1 → 3`

### Important

In a **Binary Search Tree**, inorder traversal visits values in **sorted order**.

---

## Postorder

`Left → Right → Root`

Process the current node **after** both subtrees.

Traversal:

`4 → 5 → 2 → 3 → 1`

### Recognition

Useful when a node's result depends on information calculated from its children.

Examples include:

- subtree calculations
- deleting/freeing a tree
- calculating height/depth

---

## DFS Recursive Mindset

Most recursive tree problems follow this idea:

> **Ask what information the child should return to its parent.**

For example:

```text
        1
       / \
      2   3
     /
    4
```

For a depth problem:

```text
depth(4) → 1
depth(2) → 2
depth(3) → 1
depth(1) → 3
```

The parent uses the values returned by its children.

---

## Tree Comparison / Matching

Some tree problems require comparing two trees **node by node at corresponding positions**.

For each pair of nodes:

1. Both are `null` → match.
2. Only one is `null` → mismatch.
3. Values differ → mismatch.
4. Otherwise, recursively compare both left and right subtrees.

Both subtree comparisons must be valid.

This pattern is used in:

- Same Tree
- Subtree of Another Tree

**Key Insight**

> Many tree comparison problems can be solved by recursively asking whether the corresponding subtrees satisfy the same condition.

---

## Bottom-Up Recursive Aggregation

Some tree problems require information from a node's children before the parent can be solved.

The recursion therefore works **bottom-up**:

```text
Left subtree
      ↓
Right subtree
      ↓
Current node
      ↓
Return information to parent
```

Examples:

- Maximum Depth → children return subtree heights.
- Balanced Binary Tree → children return heights while also detecting imbalance.

### Sentinel Value Technique

A recursive helper can return a special value to signal an invalid subtree.

For example:

```text
positive value → valid subtree + useful information
-1             → subtree is invalid
```

In **Balanced Binary Tree**, `-1` means the subtree is unbalanced.

**Key Insight**

> A recursive function can return both useful information and a special signal indicating that the subtree has already failed a condition.

---

## Constraint / Range Propagation

Some tree problems require a node to satisfy constraints imposed by **multiple ancestors**, not just its parent.

Pass the valid range down through the recursion:

```text
Root:
(-∞, +∞)

Left child:
(-∞, root.val)

Right child:
(root.val, +∞)
```

As recursion moves deeper, the valid range becomes narrower.

This is used in:

- Validate Binary Search Tree

For each node:

`leftBound < node.val < rightBound`

### Key Insight

> When a node's validity depends on its position in the entire tree, carry the relevant constraints down through the recursive calls.

---

# BFS — Breadth-First Search

BFS explores the tree **level by level**.

Example:

```text
        1
       / \
      2   3
     / \   \
    4   5   6
```

Traversal:

`1 → 2 → 3 → 4 → 5 → 6`

BFS is typically implemented using a **Queue**.

### Level Order

BFS is commonly called **level-order traversal** because it processes:

```text
Level 1 → Level 2 → Level 3 → ...
```

### Recognition

Think about BFS when the problem involves:

- level-by-level processing
- nodes at a particular depth
- shortest path in an unweighted tree
- finding the nearest/closest node satisfying a condition

---

# DFS vs BFS

| DFS | BFS |
|---|---|
| Goes deep before backtracking | Goes level by level |
| Usually recursion or stack | Usually queue |
| Preorder / Inorder / Postorder | Level-order |
| Good for subtree/recursive problems | Good for level/distance problems |

---

## Complexity

For a tree with `n` nodes:

### Traversal Time

`O(n)`

Every node is visited once.

### Space

DFS recursion/stack:

`O(h)`

where `h` is the tree height.

BFS queue:

`O(w)`

where `w` is the maximum width of the tree.

For DFS:

- Balanced tree → `O(log n)`
- Skewed tree → `O(n)`

---

## Key Recognition Questions

When given a tree problem, ask:

1. **Do I need to explore subtrees recursively?**  
   → Think **DFS**.

2. **Does the problem depend on levels or distances from the root?**  
   → Think **BFS**.

3. **When should I process the current node?**
   - Before children → **Preorder**
   - Between children → **Inorder**
   - After children → **Postorder**

4. **What information should a child return to its parent?**  
   → This often reveals the recursive solution.

## Core Pattern

```text
Tree
├── DFS
│   ├── Preorder
│   ├── Inorder
│   └── Postorder
│
└── BFS
    └── Level Order
```