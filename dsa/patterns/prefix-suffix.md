# Prefix & Suffix Pattern

## 💡 Core Idea

Instead of recalculating values for every index, precompute information from:

- Left (Prefix)
- Right (Suffix)

Then combine them.

```
Answer = Prefix × Suffix
```

This avoids repeated work and often reduces the time complexity from **O(n²)** to **O(n)**.

---

## 🧠 When to Recognize This Pattern

Look for questions like:

- Product of all elements except itself
- Sum of all elements except itself
- Left/right cumulative values
- Range calculations
- Excluding the current element
- Need information from both left and right

---

## ✅ General Approach

### Step 1

Compute all prefix values.

```
Prefix[i] = Product/Sum of everything before i
```

---

### Step 2

Compute all suffix values.

```
Suffix[i] = Product/Sum of everything after i
```

---

### Step 3

Combine them.

```
Answer[i] = Prefix[i] × Suffix[i]
```

or

```
Answer[i] = Prefix[i] + Suffix[i]
```

depending on the problem.

---

## 🚀 Space Optimization

Instead of storing both arrays,

- Store prefix values directly inside the result array.
- Traverse from right to left while maintaining one running suffix variable.

```
result = Prefix

↓

Multiply each element by running suffix
```

This reduces auxiliary space to **O(1)**.

---

## ⏱ Complexity

Basic Prefix + Suffix

Time: O(n)

Space: O(n)

---

Optimized Prefix + Running Suffix

Time: O(n)

Space: O(1)

---

## ⚠ Common Mistakes

❌ Forgetting that there are no elements before the first index.

```
Prefix[0] = 1
```

---

❌ Forgetting that there are no elements after the last index.

```
Suffix[last] = 1
```

---

❌ Off-by-one indexing.

Remember:

```
Prefix uses previous element.

Suffix uses next element.
```

---

## 📚 Problems Using This Pattern

- Product of Array Except Self
- Trapping Rain Water (variation)
- Prefix Sum problems
- Range Query problems