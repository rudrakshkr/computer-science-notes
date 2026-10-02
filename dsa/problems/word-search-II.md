# LeetCode 212 — Word Search II

## Pattern
**Trie + DFS + Backtracking + Pruning**

## Core Idea

Instead of searching for every word separately:

1. Insert all words into a Trie.
2. Start DFS from every cell of the board.
3. Traverse the Trie and board simultaneously.
4. Stop immediately when the current character does not exist in the Trie.
5. When a Trie node contains a complete word, add it to the result.
6. Mark the current board cell as visited.
7. Explore all 4 directions.
8. Restore the cell while backtracking.

The Trie allows multiple words with shared prefixes to be searched together.

---

## Trie Structure

Each Trie node stores:

- `children` → character → next Trie node
- `word` → complete word when this node represents the end of one

Using `word` means a separate `isEnd` property is not necessary.

Example:

    root
     ├── o
     │    └── a
     │         └── t
     │              └── h → "oath"
     │
     └── e
          └── a
               └── t → "eat"

---

## DFS State

    dfs(row, col, trieNode)

- `row`, `col` → current board position
- `trieNode` → current position in the Trie

At each cell:

    board character
          ↓
    check Trie child
          ↓
    move to child
          ↓
    check whether a word ends here
          ↓
    explore 4 directions

---

## Pruning

If the current board character does not exist as a child of the current Trie node:

    stop DFS

This prevents exploring paths that cannot form any word.

For a `Map` Trie:

    if (!node.children.has(char)) return;

---

## Finding a Word

When:

    node.word !== null

the current board path forms a complete word.

Add it to the result:

    result.push(node.word);

Then:

    node.word = null;

This prevents the same word from being added multiple times.

Do **not** stop DFS after finding a word, because a longer word may continue from the same Trie node.

Example:

    "oat"
    "oath"

Finding `"oat"` does not mean `"oath"` cannot also be found.

---

## Backtracking

A board cell cannot be reused in the same path.

Save the original character:

    const char = board[r][c];

Mark it as visited:

    board[r][c] = "#";

Explore the four directions.

Then restore the original character:

    board[r][c] = char;

Pattern:

    choose
      ↓
    explore
      ↓
    undo choice

---

## Four Directions

From `(r, c)`:

    dfs(r + 1, c, node); // down
    dfs(r - 1, c, node); // up
    dfs(r, c + 1, node); // right
    dfs(r, c - 1, node); // left

---

## Why Trie Is Useful

Without a Trie, we would search the board separately for every word:

    word 1 → DFS
    word 2 → DFS
    word 3 → DFS
    ...

This can repeatedly explore the same prefixes.

With a Trie:

    all words
       ↓
    shared prefixes
       ↓
    one search structure
       ↓
    prune invalid prefixes immediately

---

## Duplicate Prevention

A word may be discovered through multiple DFS starting points.

Instead of maintaining a separate `Set`, store the complete word in the Trie node:

    if (node.word !== null) {
        res.push(node.word);
        node.word = null;
    }

Setting `word = null` after finding it ensures that word is added only once.

---

## Algorithm

    1. Create the Trie root.
    2. Insert every word into the Trie.
    3. For every board cell:
           run DFS(cell, root)
    4. In DFS:
           - reject out-of-bounds cells
           - reject already visited cells
           - reject characters missing from the Trie
           - move to the Trie child
           - record the word if one ends here
           - mark the board cell as visited
           - explore 4 directions
           - restore the board cell
    5. Return the result.

---

## Complexity

Let:

- `L` = total number of characters across all words
- `R × C` = board dimensions
- `K` = maximum word length

Trie construction:

    O(L)

DFS worst case:

    O(R × C × 4^K)

The Trie significantly reduces unnecessary searches through prefix pruning.

Space:

    O(L + K)

excluding the output.

---

## Recognition

Think **Trie + DFS + Backtracking** when:

- You need to find multiple words.
- Words share common prefixes.
- The input is a 2D character board.
- Movement is between neighboring cells.
- A cell cannot be reused in the same path.
- Invalid prefixes can be rejected early.