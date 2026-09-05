# LRU Cache — LeetCode 146

## LRU & MRU

- **LRU (Least Recently Used)** → the entry that has been unused for the longest time. This is the first entry to be evicted when the cache is full.
- **MRU (Most Recently Used)** → the entry that was accessed or inserted most recently.

Example:

`LEFT ⇄ A ⇄ B ⇄ C ⇄ RIGHT`

Here:

- `A` = LRU
- `C` = MRU

## Core Idea

Use two data structures together:

- **HashMap** → stores `key → Node` for O(1) lookup.
- **Doubly Linked List** → maintains the usage order for O(1) removal and insertion.

Order is maintained as:

`LEFT ⇄ LRU ⇄ ... ⇄ MRU ⇄ RIGHT`

`LEFT` and `RIGHT` are dummy/sentinel nodes.

## Why Doubly Linked List?

When a key is accessed, its Node must move to the MRU position.

A doubly linked list allows us to:

- remove a Node from anywhere in O(1)
- insert a Node at the MRU end in O(1)
- directly access the LRU node using `LEFT.next`

The Node stores both `key` and `value`, along with `prev` and `next`.

## `get(key)` Algorithm

1. Check whether the key exists in the Map.
2. If not → return `-1`.
3. Get the corresponding Node.
4. Remove the Node from its current position.
5. Insert it immediately before `RIGHT` → it becomes MRU.
6. Return its value.

## `put(key, value)` Algorithm

### Existing key

1. Get the existing Node from the Map.
2. Update its value.
3. Remove it from its current position.
4. Move it to MRU.

### New key + cache not full

1. Create a new Node.
2. Store it in the Map.
3. Insert it at MRU.

### New key + cache full

1. Find the LRU node: `LEFT.next`.
2. Remove it from the linked list.
3. Delete its key from the Map.
4. Create the new Node.
5. Store it in the Map.
6. Insert it at MRU.

## Example

Capacity = `2`

After:

`put(A,1), put(B,2)`

```text
LEFT ⇄ A ⇄ B ⇄ RIGHT
       LRU  MRU
```

After `get(A)`:

```text
LEFT ⇄ B ⇄ A ⇄ RIGHT
       LRU  MRU
```

Now `put(C,3)` evicts `B` because it is the LRU:

```text
LEFT ⇄ A ⇄ C ⇄ RIGHT
       LRU  MRU
```

## Complexity

- `get()` → O(1)
- `put()` → O(1)
- Space → O(capacity)

## Key Interview Takeaway

**LRU Cache = HashMap + Doubly Linked List**

- Map → **find the Node quickly**
- Linked List → **maintain and modify recency order quickly**