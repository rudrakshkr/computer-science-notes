# Trie Pattern

## Core Idea

A **Trie** (prefix tree) is a tree-like data structure designed to store and search strings by their characters.

Words that share a prefix share the same path.

Example:

```text
apple
app
ape
bat
```

Conceptually:

```text
root
├── a
│   └── p
│       ├── p
│       │   └── l
│       │       └── e
│       └── e
└── b
    └── a
        └── t
```

The main advantage is efficient **prefix-based operations**.

---

## Trie Node

A Trie node generally contains:

```text
children
isEnd
```

### `children`

Stores references to the next character nodes.

In JavaScript, this can be implemented using:

```js
this.children = {};
```

or:

```js
this.children = new Map();
```

### `isEnd`

Indicates whether a complete word ends at this node.

This is necessary because one word can be a prefix of another.

Example:

```text
app
apple
```

Both share:

```text
a → p → p
```

The second `p` must have:

```js
isEnd = true;
```

after `app` is inserted.

---

## Why We Don't Need to Store the Character

A node does not necessarily need a separate `value` property.

The character can be identified through its parent's `children`.

For example:

```js
node.children["a"]
```

already tells us that this child represents `a`.

Therefore:

```text
TrieNode
├── children
└── isEnd
```

is enough.

---

## Core Operations

### Insert

For every character:

1. Start at the root.
2. Check whether the character already exists.
3. Create a node if it doesn't exist.
4. Move to that child.
5. After the final character, mark `isEnd = true`.

Conceptually:

```text
current = root

for each character:
    if character doesn't exist:
        create node
    move to child

mark current as end of word
```

---

### Search

To search for a complete word:

1. Traverse the characters.
2. If a required child doesn't exist, return `false`.
3. After reaching the final character, check `isEnd`.

Important:

```text
path exists ≠ word exists
```

A path may only represent a prefix.

---

### Starts With

To check whether a prefix exists:

1. Traverse the characters.
2. If any character is missing, return `false`.
3. If the entire prefix can be traversed, return `true`.

`isEnd` does not matter here.

---

## Important Distinction

Suppose only:

```text
apple
```

has been inserted.

Then:

```text
search("app")      → false
startsWith("app")  → true
```

because the path:

```text
a → p → p
```

exists, but `app` itself was never marked as a complete word.

After inserting:

```text
app
```

then:

```text
search("app")      → true
startsWith("app")  → true
```

---

## Shared Traversal Pattern

A useful abstraction is a traversal helper:

```js
_traverse(word) {
    let node = this.root;

    for (let char of word) {
        if (!node.children[char]) {
            return null;
        }

        node = node.children[char];
    }

    return node;
}
```

Then:

```text
search
→ traverse + check isEnd

startsWith
→ traverse only
```

This avoids duplicating traversal logic.

---

## Complexity

Let `L` be the length of the word or prefix.

### Insert

```text
O(L)
```

### Search

```text
O(L)
```

### Starts With

```text
O(L)
```

### Space

```text
O(total number of characters stored)
```

The exact memory usage depends on how many nodes are created and how `children` is represented.

---

## Recognition Questions

Consider using a Trie when the problem involves:

```text
Prefix queries
String insertion/search
Autocomplete
Dictionary-like word lookup
Shared prefixes
Wildcard character traversal
Word searching across prefixes
```

Typical clues:

```text
"starts with"
"prefix"
"dictionary"
"insert words"
"search words"
"autocomplete"
```

---

## Core Mental Model

Remember:

```text
Trie
→ characters become nodes

children
→ how to continue the word

isEnd
→ whether a complete word ends here
```