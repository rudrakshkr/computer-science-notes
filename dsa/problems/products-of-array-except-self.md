# Product of Array Except Self

## 🟢 Problem

Return an array where:

```
answer[i] = Product of all elements except nums[i]
```

Division is **not allowed**.

---

## 💡 Key Insight

Every answer is:

```
Left Product × Right Product
```

Example

```
nums   = [1,2,3,4]

Left   = [1,1,2,6]

Right  = [24,12,4,1]

Answer = [24,12,8,6]
```

---

## 🚀 Optimal Approach

### Pass 1

Store prefix products directly inside the result array.

```
result = Prefix
```

### Pass 2

Traverse from right to left using one variable.

```
rightProduct = 1

result[i] *= rightProduct

rightProduct *= nums[i]
```

No suffix array needed.

---

## ⏱ Complexity

Time: **O(n)**

Space: **O(1)**

(Output array not counted.)

---

## ⚠ Common Mistakes

- Prefix begins with `1`
- Suffix ends with `1`
- Multiply first, then update `rightProduct`

---

## 📌 Pattern

**Prefix & Suffix**