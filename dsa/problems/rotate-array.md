# LeetCode 189 — Rotate Array

## Problem

Given an integer array `nums`, rotate the array **to the right by `k` steps**.

### Example

**Input:**

```text
nums = [1,2,3,4,5,6,7]
k = 3
```

**Output:**

```text
[5,6,7,1,2,3,4]
```

---

## Key Idea

A right rotation by `k` means moving the last `k` elements to the front.

```text
[1,2,3,4 | 5,6,7]
             ↓
[5,6,7 | 1,2,3,4]
```

The standard in-place solution uses the **reversal algorithm**.

---

# Reversal Algorithm

For:

```text
nums = [1,2,3,4,5,6,7]
k = 3
```

### Step 1 — Reverse the entire array

```text
[1,2,3,4,5,6,7]
        ↓
[7,6,5,4,3,2,1]
```

### Step 2 — Reverse the first `k` elements

```text
[7,6,5 | 4,3,2,1]
   ↓↓↓
[5,6,7 | 4,3,2,1]
```

### Step 3 — Reverse the remaining elements

```text
[5,6,7 | 4,3,2,1]
          ↓──────
[5,6,7 | 1,2,3,4]
```

Final result:

```text
[5,6,7,1,2,3,4]
```

---

# Why Does This Work?

Split the original array into two parts:

```text
[A | B]
```

Where:

- `A` = first `n-k` elements
- `B` = last `k` elements

We want:

```text
[B | A]
```

### Reverse the entire array

```text
[A | B]
```

becomes:

```text
[reverse(B) | reverse(A)]
```

Then reverse each part:

```text
[reverse(reverse(B)) | reverse(reverse(A))]
```

which gives:

```text
[B | A]
```

Therefore, the array is rotated correctly.

---

# Important: `k` Can Be Larger Than `n`

For example:

```text
nums = [1,2,3,4,5]
k = 7
```

Rotating 7 times is equivalent to rotating:

```text
7 % 5 = 2
```

So always do:

```javascript
k = k % nums.length;
```

---

# JavaScript Solution

```javascript
var rotate = function(nums, k) {
    k = k % nums.length;

    function reverse(left, right) {
        while (left < right) {
            [nums[left], nums[right]] =
                [nums[right], nums[left]];

            left++;
            right--;
        }
    }

    // Reverse the entire array
    reverse(0, nums.length - 1);

    // Reverse the first k elements
    reverse(0, k - 1);

    // Reverse the remaining elements
    reverse(k, nums.length - 1);
};
```

---

# Dry Run

Given:

```text
nums = [1,2,3,4,5,6,7]
k = 3
```

### Original

```text
[1,2,3,4,5,6,7]
```

### Reverse entire array

```text
[7,6,5,4,3,2,1]
```

### Reverse first `k = 3`

```text
[5,6,7,4,3,2,1]
```

### Reverse remaining elements

```text
[5,6,7,1,2,3,4]
```

---

# Complexity

## Time Complexity

```text
O(n)
```

We reverse the array three times:

```text
O(n) + O(n) + O(n) = O(n)
```

## Space Complexity

```text
O(1)
```

The algorithm modifies the original array in-place and does not create another array.

---

# Interview Pattern

Remember:

```text
Right Rotate by k

1. k = k % n
2. Reverse the entire array
3. Reverse the first k elements
4. Reverse the remaining elements
```

---

## Key Distinction: Right Rotation vs Left Rotation

### Right Rotation

Moves the **last `k` elements to the front**.

Example:

```text
[1,2,3,4,5], k = 2
→ [4,5,1,2,3]
```

Reversal order:

```text
1. Reverse the entire array
2. Reverse the first k elements
3. Reverse the remaining elements
```

---

### Left Rotation

Moves the **first `k` elements to the end**.

Example:

```text
[1,2,3,4,5], k = 2
→ [3,4,5,1,2]
```

Reversal order:

```text
1. Reverse the first k elements
2. Reverse the remaining elements
3. Reverse the entire array
```