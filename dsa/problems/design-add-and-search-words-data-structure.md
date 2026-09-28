# LeetCode 211 — Design Add and Search Words Data Structure

## Approach

Build a Trie similar to LeetCode 208, but `search()` must support `.`.

Each Trie node stores:

```text
children
isEnd
```

### `addWord(word)`

Same idea as a normal Trie:

- Traverse each character.
- Create missing nodes.
- Move to the child.
- Mark the final node with `isEnd = true`.

### `search(word)`

Use **DFS** with:

```text
node → current Trie node
i    → current index in the search word
```

For each character:

- Normal character → follow that specific child.
- `.` → try **every child** because `.` can represent any character.
- If any `.` branch succeeds, return `true`.
- When `i === word.length`, return `node.isEnd`.

## Key Insight

```text
Normal character
→ one possible path

'.'
→ multiple possible paths
→ DFS through each child
```

A matching path is not enough; the final node must also have:

```text
isEnd = true
```

## Complexity

Let `L` be the word length.

### `addWord`

```text
O(L)
```

### `search`

- Without `.`: `O(L)`
- With `.`: can branch through many Trie paths; worst case is exponential in `L`.

### Space

```text
O(total characters stored)
```

plus the DFS recursion stack.

## Core Pattern

This problem combines:

```text
Trie
+
DFS / Backtracking
```

The important upgrade from LeetCode 208 is:

```text
208 → deterministic Trie traversal
211 → Trie traversal + branching when encountering '.'
```