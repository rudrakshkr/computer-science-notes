# Linked List — Pointer Manipulation

## Core Idea

A **Linked List** consists of nodes where each node stores:

- A value
- A reference to the next node

Unlike arrays, nodes are not necessarily stored contiguously in memory.

---

## Key Recognition Question

Ask:

> **Do I need to traverse nodes and manipulate their connections or `next` pointers?**

If yes, think about **Linked List pointer manipulation**.

---

## Pointer Traversal

For a singly linked list, use a pointer such as:

`current`

to move through the list.

Conceptually:

`current → current.next → current.next.next → ...`

Continue until:

`current === null`

---

## Pointer Manipulation

When changing links between nodes, be careful not to lose access to the rest of the list.

### Important Rule

> **Save the next node before modifying the current node's pointer.**

General sequence:

1. Save the next node.
2. Modify the current node's pointer.
3. Move the previous pointer forward.
4. Move the current pointer forward.

---

## Multiple Pointer Technique

Many linked list problems require maintaining multiple pointers.

Common pointers include:

- `prev` → previous node
- `current` → current node
- `next` → saved next node

Each pointer represents a different position or piece of information during traversal.

---

## Why Multiple Pointers Matter

Suppose:

`1 → 2 → 3`

If you change:

`1.next = null`

without first saving node `2`, you lose access to the remainder of the list.

Therefore:

`next = current.next`

must happen before modifying `current.next`.

---

## Slow/Fast Pointer Technique

Use two pointers moving through the linked list at different speeds.

- `slow` → moves 1 node at a time
- `fast` → moves 2 nodes at a time

This is useful for:

- Detecting cycles
- Finding the middle of a linked list
- Other problems involving relative pointer movement

### Cycle Detection

If the list contains a cycle:

`slow === fast`

because the faster pointer will eventually catch the slower pointer.

If no cycle exists:

`fast` eventually reaches `null`.

### Important Detail

Compare **node references**, not node values.

Use:

`slow === fast`

not:

`slow.val === fast.val`

Two different nodes can contain the same value.

---

## Complexity

For a single traversal through `n` nodes:

Time: `O(n)`

If only a constant number of pointers are used:

Space: `O(1)`