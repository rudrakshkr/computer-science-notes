# Longest Consecutive Sequence

## 💡 Pattern

**HashSet**

Use a Set for **O(1) average lookup** and avoid sorting.

---

## Approach

Given:

```js
nums = [100, 4, 200, 1, 3, 2]
```

Create a Set:

```js
const set = new Set(nums);
```

### 1. Find the start of a sequence

For every number, check whether its previous number exists:

```js
if (set.has(num - 1)) continue;
```

If `num - 1` exists, `num` is **not** the beginning of a sequence.

Example:

```text
1 → start
2 → 1 exists, skip
3 → 2 exists, skip
4 → 3 exists, skip
```

---

### 2. Count the sequence

Once we find the start:

```js
let count = 1;

while (set.has(num + 1)) {
    count++;
    num++;
}
```

For:

```text
1 → 2 → 3 → 4
```

`count = 4`.

---

### 3. Track the longest sequence

```js
longestCount = Math.max(longestCount, count);
```

---

## Solution

```js
const set = new Set(nums);
let longestCount = 0;

for (let num of set) {
    // Not the start of a sequence
    if (set.has(num - 1)) continue;

    let count = 1;

    // Count consecutive numbers
    while (set.has(num + 1)) {
        count++;
        num++;
    }

    longestCount = Math.max(longestCount, count);
}

return longestCount;
```

---

## Why HashSet?

We need to repeatedly ask:

> "Does the next number exist?"

A Set gives **O(1) average lookup**:

```js
set.has(num)
```

So we don't need to scan the array.

---

## Why don't we sort?

Sorting:

```js
nums.sort((a, b) => a - b);
```

costs:

```text
O(n log n)
```

The Set approach achieves:

```text
O(n)
```

average time.

---

## Complexity

**Time:** `O(n)` average

**Space:** `O(n)`

---

## Key Insight ⭐

Don't start counting from every number.

Only start when:

```js
!set.has(num - 1)
```

This means:

> **`num` is the beginning of a consecutive sequence.**

Then count forward using:

```js
set.has(num + 1)
```

---

### Interview takeaway

**HashSet + sequence-start detection = O(n) Longest Consecutive Sequence**