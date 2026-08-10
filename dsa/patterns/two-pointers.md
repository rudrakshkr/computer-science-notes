# Two Pointers

## Definition

The **Two Pointers** technique uses two indices/pointers to traverse a data structure, usually from opposite ends or at different speeds, to avoid unnecessary nested loops.

## When to Use

Commonly useful when:

- Working with arrays or strings
- Comparing elements from opposite ends
- Looking for pairs
- The input is sorted
- You need to reduce `O(n²)` comparisons

## Common Approach

### Opposite Ends

```js
let left = 0;
let right = arr.length - 1;

while (left < right) {
    // compare/process arr[left] and arr[right]

    left++;
    right--;
}
```

### Key Idea

Instead of checking every possible pair:

```text
O(n²)
```

move the pointers intelligently:

```text
left → →     ← ← right
```

giving:

```text
O(n)
```

## Example

**Valid Palindrome**

Compare characters from both ends:

```text
a b c b a
↑       ↑
L       R
```

If they match:

```text
L →     ← R
```

Continue until the pointers meet.

## Complexity

Usually:

**Time:** `O(n)`

**Space:** `O(1)`

## ⭐ Interview Takeaway

> Use Two Pointers when you can make progress from two positions simultaneously instead of repeatedly scanning the same elements.