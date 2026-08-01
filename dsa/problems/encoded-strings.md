# Encode & Decode Strings

**Difficulty:** Medium

---

## Pattern

String Manipulation

---

## Intuition

Using a separator alone is ambiguous because the separator may exist inside the original string.

Instead, store the **length** of each string before the string itself.

Format:

```text
length#string
```

Example:

```text
4#neet4#code
```

---

## Algorithm

### Encode

1. For each string:
   - Get its length.
   - Append `length#string`.

### Decode

1. Find `#`.
2. Characters before `#` represent the string length.
3. Read exactly `length` characters.
4. Repeat until the end.

---

## Why This Works

The decoder never splits by `#`.

It first reads the length, then reads exactly that many characters.

So special characters inside the string do not cause any ambiguity.

---

## Complexity

### Time

- Encode: **O(total characters)**
- Decode: **O(total characters)**

### Space

**O(total characters)**