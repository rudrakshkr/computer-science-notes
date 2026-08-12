# Two Sum II — Input Array Is Sorted

## 💡 Core Idea

Use **Two Pointers** because the array is already sorted.

```text
LEFT →                 ← RIGHT
[2, 7, 11, 15]
```

Calculate:

```text
sum = numbers[LEFT] + numbers[RIGHT]
```

---

## 🧠 Pointer Movement

- If `sum < target` → move `LEFT` right to increase the sum.
- If `sum > target` → move `RIGHT` left to decrease the sum.
- If `sum === target` → return the two indices.

The sorted order allows us to eliminate unnecessary pairs without checking every combination.

---

## ⏱ Complexity

**Time:** `O(n)`

Each pointer only moves in one direction.

**Space:** `O(1)`

Only two pointers and a few variables are used.

---

## ⚠ Common Mistake

Using nested loops:

```text
for → while
```

results in **O(n²)** because the inner pointer starts over for every `i`.

---

## ⭐ Key Takeaway

> A sorted array allows two pointers to eliminate possible pairs without checking every combination.