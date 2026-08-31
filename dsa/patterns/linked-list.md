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

## Dummy Node Technique

A **dummy node** is a temporary node placed before the head of the linked list.

Conceptually:

`dummy → head → ...`

It simplifies pointer manipulation, especially when the operation may modify or remove the **head node**.

### Common Uses

- Removing the head node
- Removing a node near the head
- Building a new linked list
- Simplifying edge cases involving the first node

At the end, the actual head is usually:

`dummy.next`

---

## Two-Pointer Gap Technique

Maintain two pointers with a fixed distance between them.

This is useful when a problem asks about a node relative to the **end** of the list.

For example, to find the nth node from the end:

1. Place `slow` and `fast` at the dummy node.
2. Move `fast` ahead by `n + 1` nodes.
3. Move both pointers together.
4. When `fast === null`, `slow` is positioned immediately before the nth node from the end.

### Key Insight

> Maintain a fixed gap between two pointers so that reaching the end with one pointer gives useful positional information about the other.

---

## Complexity

For a single traversal through `n` nodes:

Time: `O(n)`

If only a constant number of pointers are used:

Space: `O(1)`