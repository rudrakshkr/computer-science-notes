# LeetCode 46 — Permutations

**Pattern:** Backtracking  
**Difficulty:** Medium  
**Topic:** Recursion, State Tracking, Permutations

## 1. Problem Summary

Given an array `nums` containing distinct integers, return all possible permutations of the array.

A permutation is an arrangement of elements where **the order matters**.

Example:

```text
Input: nums = [1, 2, 3]

Output:
[
    [1, 2, 3],
    [1, 3, 2],
    [2, 1, 3],
    [2, 3, 1],
    [3, 1, 2],
    [3, 2, 1]
]
```

For `n` distinct elements, there are `n!` possible permutations.

---

## 2. Core Idea

Use backtracking to construct each permutation one position at a time.

At every position:

1. Iterate through all input elements.
2. Skip elements that are already used in the current permutation.
3. Choose an unused element.
4. Mark it as used.
5. Recursively fill the next position.
6. Undo the choice to explore other possibilities.

The key pattern is:

**Choose → Mark Used → Recurse → Undo → Unmark**

Unlike Subsets, where each element has an include/exclude decision, Permutations requires choosing which unused element comes next.

---

## 3. State Variables

| Variable | Purpose |
|---|---|
| `nums` | Input array |
| `result` | Stores all completed permutations |
| `current` | Permutation currently being constructed |
| `used[i]` | Indicates whether the element at index `i` is already in `current` |
| `index` | Number of positions already filled in the current permutation |
| `i` | Iterates through the input to find an unused element |

### Important distinction

`index` and `i` serve different purposes:

- `index` identifies the position being filled in the permutation.
- `i` identifies a candidate element from the input array.

Every recursive call advances `index`, while the loop searches all input positions for an unused element.

---

## 4. Base Case

```js
if (index === nums.length) {
    result.push([...current]);
    return;
}
```

When `index` reaches the array length, every position has been filled.

At this point:

- `current` contains a complete permutation.
- A copy is stored in `result`.
- The recursive call returns.

Use `[...current]` because `current` is a mutable array that will be modified as backtracking continues.

---

## 5. Choosing an Unused Element

```js
for (let i = 0; i < nums.length; i++) {
    if (used[i]) continue;

    current.push(nums[i]);
    used[i] = true;

    backtrack(index + 1);

    current.pop();
    used[i] = false;
}
```

### Step 1: Skip used elements

```js
if (used[i]) continue;
```

An element already present in the current permutation cannot be selected again.

### Step 2: Choose the element

```js
current.push(nums[i]);
used[i] = true;
```

Add the selected number to the permutation and mark its input position as used.

### Step 3: Explore recursively

```js
backtrack(index + 1);
```

Move to the next position and select another unused element.

### Step 4: Undo the choice

```js
current.pop();
used[i] = false;
```

Remove the selected number and mark it as available again.

This restores the previous state so the loop can try a different candidate.

---

## 6. Complete JavaScript Solution

```js
var permute = function(nums) {
    const result = [];
    const current = [];
    const used = Array(nums.length).fill(false);

    function backtrack(index) {
        if (index === nums.length) {
            result.push([...current]);
            return;
        }

        for (let i = 0; i < nums.length; i++) {
            if (used[i]) continue;

            // Choose
            current.push(nums[i]);
            used[i] = true;

            // Explore
            backtrack(index + 1);

            // Undo
            current.pop();
            used[i] = false;
        }
    }

    backtrack(0);

    return result;
};
```

---

## 7. Example Walkthrough

Input:

```text
nums = [1, 2, 3]
```

Initially:

```text
current = []
used    = [false, false, false]
index   = 0
```

### First branch: Choose 1

```text
current = [1]
used    = [true, false, false]
index   = 1
```

The remaining choices are `2` and `3`.

Choose `2`:

```text
current = [1, 2]
used    = [true, true, false]
index   = 2
```

Choose `3`:

```text
current = [1, 2, 3]
used    = [true, true, true]
index   = 3
```

The base case is reached. Save `[1, 2, 3]`.

Backtrack by removing `3`:

```text
current = [1, 2]
used    = [true, true, false]
```

No other unused element remains at this position, so return and undo `2`.

```text
current = [1]
used    = [true, false, false]
```

Now choose `3` instead of `2`.

```text
current = [1, 3]
```

The only remaining element is `2`, producing `[1, 3, 2]`.

Both permutations beginning with `1` have now been generated.

### Remaining branches

The algorithm then backtracks to the empty permutation and chooses `2` first, followed by `3`.

It generates:

```text
[2, 1, 3]
[2, 3, 1]
[3, 1, 2]
[3, 2, 1]
```

The complete result contains six permutations, matching:

`3! = 3 × 2 × 1 = 6`

---

## 8. Why the `used` Array Is Necessary

Without tracking used elements, the algorithm could select the same element multiple times in one permutation.

For example, it might incorrectly generate:

```text
[1, 1, 1]
```

The `used` array prevents this by recording which input positions are already selected along the current recursive path.

The array must be restored during backtracking:

```js
used[i] = true;

backtrack(index + 1);

used[i] = false;
```

The element is unavailable in the current branch but available again when exploring another branch.

Because the input contains distinct integers, tracking input positions is sufficient.

---

## 9. Complexity Analysis

Let `n` be the number of input elements.

### Time Complexity: `O(n × n!)`

There are `n!` permutations.

Each completed permutation requires `O(n)` time to copy into the result. The recursive search and candidate checks are also bounded by `O(n × n!)`.

### Auxiliary Space Complexity: `O(n)`

The recursion depth is at most `n`. The `current` array and `used` array each require `O(n)` space.

### Output Space Complexity: `O(n × n!)`

There are `n!` permutations, each containing `n` elements.

Output space is separate from auxiliary space.

---

## 11. Subsets vs. Permutations

| Subsets | Permutations |
|---|---|
| Order does not matter | Order matters |
| Each element is included or excluded | Each position selects an unused element |
| Common approach: include/exclude or a loop | Common approach: loop + used tracking |
| `n` elements produce `2^n` subsets | `n` distinct elements produce `n!` permutations |
| Often tracks an index or start position | Often tracks a `used` array |

---

## Key Takeaway

**Permutations uses backtracking to fill each position by choosing an unused element, recursively exploring the remaining choices, and undoing each selection before trying another.**