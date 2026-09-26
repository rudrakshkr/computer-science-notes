# LeetCode 208 — Implement Trie

## Approach

Build a Trie where each node stores:

- `children` → next characters
- `isEnd` → whether a complete word ends at this node

### Operations

**Insert**
- Traverse each character.
- Create missing nodes.
- Mark the final node as `isEnd = true`.

**Search**
- Traverse the word.
- Return `true` only if the path exists **and** the final node is an end of a word.

**StartsWith**
- Traverse the prefix.
- Return `true` if the entire path exists.

## Key Insight

```text
Path exists
→ prefix exists

Path exists + isEnd
→ complete word exists
```

Shared prefixes reuse the same nodes.

MORE INFORMATION ABOUT THIS PROBLEM CAN BE FOUND IN `./patterns/tries.md`

## Complexity

For a word/prefix of length `L`:

```text
Insert     → O(L)
Search     → O(L)
StartsWith → O(L)
```

Space:

```text
O(total characters stored)
```