# Two Pointers Pattern

## Definition

The **Two Pointers** technique uses two indices/pointers to traverse a data structure, usually from opposite ends or at different speeds, to avoid unnecessary nested loops.

## 🧠 When to Recognize This Pattern

Commonly useful when:

- Comparing elements from opposite ends
- Finding pairs in a sorted array
- Finding combinations of multiple elements
- Checking whether elements satisfy a condition from both sides
- Problems where moving a pointer can eliminate unnecessary comparisons
- Need to reduce nested loops

---

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


## ⭐ Interview Takeaway

> Use Two Pointers when you can make progress from two positions simultaneously instead of repeatedly scanning the same elements.

## ✅ Common Approaches

### Opposite Ends

Use when comparing or processing elements from both ends of an array or string.

```text
left → →     ← ← right
```

Move both pointers toward the center based on the problem's condition.

---

### Fixed Pointer + Two Pointers

Use when finding a combination of three or more elements.

1. Sort the array.
2. Fix one element using `i`.
3. Set `left = i + 1`.
4. Set `right = end`.
5. Use `left` and `right` to search for the remaining elements.

```text
  i
  ↓
[-4, -1, -1, 0, 1, 2]
      ↑            ↑
    left         right
```

Because the array is sorted:

- Sum too small → move `left` right.
- Sum too large → move `right` left.
- Sum matches → record the result and move both pointers.

---

## 🚀 Duplicate Handling

When the array is sorted, duplicate values can produce the same result multiple times.

To avoid this:

- Skip duplicate values for the fixed pointer.
- After finding a valid combination, move both pointers.
- Skip left and right values that are the same as the values just used.

This prevents duplicate results without requiring another data structure to track them.

---

## ⏱ Complexity

### Opposite Ends

Time: O(n)

Space: O(1)

---

### Fixed Pointer + Two Pointers

Time: O(n²)

Space: O(1) auxiliary space, excluding the output.

---

## 📚 Problems Using This Pattern

- Valid Palindrome
- 3Sum
- 3Sum Closest
- 4Sum