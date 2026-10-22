# LeetCode 78 — Subsets

**Pattern:** Backtracking / Recursion  
**Difficulty:** Medium

## 1. Problem Summary

Given an array of unique integers, return every possible subset, including:

- The empty subset.
- Subsets containing one or more elements.
- The original array itself.

Example:

```text
Input: nums = [1, 2, 3]

Output:
[
    [1, 2, 3],
    [1, 2],
    [1, 3],
    [1],
    [2, 3],
    [2],
    [3],
    []
]
```

The order of the subsets does not matter.

For an array of length `n`, the number of possible subsets is `2^n`, because every element can either be included or excluded.

---

## 2. Core Idea

For each element, make two decisions:

1. Include the element in the current subset.
2. Exclude the element from the current subset.

Use recursion to explore both possibilities for every element.

A completed subset is stored when every element has been considered.

The key pattern is:

```text
Include current element
        ↓
Explore recursively
        ↓
Undo the choice
        ↓
Explore without the element
```

---

## 3. State Variables

| Variable | Purpose |
|---|---|
| `nums` | Input array |
| `index` | Index of the element currently being considered |
| `current` | Subset currently being constructed |
| `result` | Stores all completed subsets |

The recursive function receives the current index and the subset built so far.

---

## 4. Base Case

```js
if (index === nums.length) {
    result.push([...current]);
    return;
}
```

When `index` reaches `nums.length`, every element has received an include-or-exclude decision.

Therefore:

1. Save a copy of the current subset.
2. Return from this recursive call.

Use `[...current]` rather than `current` because the current array will be modified during subsequent backtracking.

---

## 5. Include / Exclude Logic

### Step 1: Include the current element

```js
current.push(nums[index]);
backtrack(index + 1);
```

Add the current element and recursively explore all subsets that include it.

### Step 2: Undo the choice

```js
current.pop();
```

Remove the element to restore the previous state.

### Step 3: Exclude the current element

```js
backtrack(index + 1);
```

Advance to the next element without adding the current one.

Both branches advance the index, but they explore different subsets.

---

## 6. Complete JavaScript Solution

```js
var subsets = function(nums) {
    const result = [];
    const current = [];

    function backtrack(index) {
        if (index === nums.length) {
            result.push([...current]);
            return;
        }

        // Include the current element
        current.push(nums[index]);
        backtrack(index + 1);

        // Undo the inclusion
        current.pop();

        // Exclude the current element
        backtrack(index + 1);
    }

    backtrack(0);

    return result;
};
```

This is the standard function-based format for LeetCode's JavaScript editor.

---

## 7. Walkthrough: nums = [1, 2, 3]

Initially:

```text
index   = 0
current = []
result  = []
```

### Step 1: Include 1

```text
current = [1]
index   = 1
```

Now explore the possibilities for `2`.

### Step 2: Include 2

```text
current = [1, 2]
index   = 2
```

Now explore the possibilities for `3`.

### Step 3: Include 3

```text
current = [1, 2, 3]
index   = 3
```

Since `index === nums.length`, save a copy:

```text
result = [[1, 2, 3]]
```

Return to the call at `index = 2`.

### Step 4: Exclude 3

First undo the previous choice:

```text
current.pop()

current = [1, 2]
index   = 2
```

Explore the branch that excludes `3`. The base case saves:

```text
result = [
    [1, 2, 3],
    [1, 2]
]
```

Return to the call at `index = 1`.

### Step 5: Exclude 2

Undo the inclusion of `2`:

```text
current.pop()

current = [1]
index   = 1
```

Now explore the possibilities for `3` without including `2`.

Include `3`:

```text
current = [1, 3]
```

Save `[1, 3]`.

Then undo the inclusion of `3`:

```text
current = [1]
```

Exclude `3` and save `[1]`.

The result now contains:

```text
[
    [1, 2, 3],
    [1, 2],
    [1, 3],
    [1]
]
```

All subsets containing `1` have been generated.

### Step 6: Exclude 1

Return to the initial call and undo the inclusion of `1`:

```text
current.pop()

current = []
index   = 0
```

Now explore the branch that excludes `1`.

Include `2` and then explore `3`:

```text
[2, 3]
[2]
```

Next, exclude `2` and explore `3`:

```text
[3]
[]
```

Final result:

```js
[
    [1, 2, 3],
    [1, 2],
    [1, 3],
    [1],
    [2, 3],
    [2],
    [3],
    []
]
```

There are eight subsets, matching `2^3`.

---

## 8. Why `current.pop()` Matters

The current subset is a mutable array shared between recursive calls.

Without `pop()`, a choice from one branch would remain in the state when the algorithm explored another branch.

For example, after exploring `[1, 2, 3]`, we need to remove `3` before exploring `[1, 2]`.

The sequence is:

```text
current = [1, 2, 3]
          ↓ pop()
current = [1, 2]
```

Similarly, when the algorithm finishes the branch containing `1`, it removes `1` to explore subsets that exclude it.

**Backtracking restores the state to what it was before a choice was made.**

---

## 9. Complexity Analysis

Let `n` be the number of input elements.

### Time Complexity

`O(n * 2^n)`

There are `2^n` subsets. Copying a subset can take up to `O(n)` time.

### Auxiliary Space Complexity

`O(n)`

The recursion stack reaches a depth of `O(n)`, and the current subset can contain up to `n` elements.

### Output Space Complexity

`O(n * 2^n)`

The result contains `2^n` subsets, with up to `n` elements per subset.

Output space is stated separately from auxiliary space.
---

## Key Takeaway

**Each element creates two branches: include it or exclude it. Explore the include branch, undo the choice with `pop()`, then explore the exclude branch. Save a copy of the current subset when every element has been considered.**