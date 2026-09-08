# Binary Tree Level Order Traversal — LeetCode 102

## Core Idea

Use **BFS** to process the tree one level at a time.

A **Queue** stores nodes waiting to be processed.

## Algorithm

1. If the root is `null`, return an empty result.
2. Put the root into the queue.
3. While there are unprocessed nodes:
   - Store the number of nodes currently belonging to the level.
   - Process exactly that many nodes.
   - Add each node's value to the current level.
   - Add its non-null children to the queue.
   - Add the completed level to the result.
4. Continue until all nodes are processed.

## Key Trick — `levelSize`

At the beginning of each level:

`levelSize = number of unprocessed nodes in queue`

Process exactly `levelSize` nodes.

Any children added during this process belong to the **next level**, so they are not processed until the next iteration.

## JavaScript Queue Optimization

Avoid repeatedly using `shift()` because removing the first element from an array can be `O(n)`.

Instead, maintain a `front` index:

`queue[front++]`

This makes dequeuing `O(1)`.

## Complexity

- Time → `O(n)`
- Space → `O(n)`

The queue can hold up to `O(n)` nodes, and the result itself also stores all node values.