# Valid Sudoku

## 🟢 Problem

Determine if a partially filled Sudoku board is valid.

Rules:

- Every row must contain unique digits.
- Every column must contain unique digits.
- Every 3×3 sub-box must contain unique digits.
- Ignore `"."` (empty cells).

---

## 💡 Key Insight

The problem is simply checking for **duplicates** in:

- Rows
- Columns
- 3×3 Boxes

Use a **HashSet** for each check.

---

## 🚀 Approach

### Check Rows

- Traverse every row.
- Store digits in a `Set`.
- If a digit already exists, return `false`.
- Clear the `Set` before checking the next row.

---

### Check Columns

- Traverse every column.
- Store digits in a `Set`.
- If a digit already exists, return `false`.
- Clear the `Set` before checking the next column.

---

### Check 3×3 Boxes

- Traverse each box using:

```
(0,0) (0,3) (0,6)

(3,0) (3,3) (3,6)

(6,0) (6,3) (6,6)
```

- Store digits in a `Set`.
- If a digit already exists, return `false`.
- Clear the `Set` after each box.

---

## ⏱ Complexity

Time: **O(1)**

Space: **O(1)**

*(For a fixed 9×9 board. For an n×n board, it would be O(n²).)*

---

## 📌 Pattern

**HashSet**