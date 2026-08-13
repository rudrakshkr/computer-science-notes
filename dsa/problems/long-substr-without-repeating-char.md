# Longest Substring Without Repeating Characters

## 🟡 Problem

Given a string `s`, find the length of the longest substring without repeating characters.

### Example

    Input:  "abcabcbb"
    Output: 3

The longest substring without repeating characters is `"abc"`.

---

## Pattern

**Sliding Window**

---

## 💡 Core Idea

Use two pointers, `L` and `R`, to maintain a window containing only unique characters.

Use a `Set` to track the characters currently inside the window.

- `R` expands the window.
- If `s[R]` is already in the Set, move `L` forward.
- Delete characters from the Set while the duplicate still exists.
- Add `s[R]` once the window becomes valid.
- Track the maximum window length.

## Algorithm

    let L = 0;
    let R = 0;
    let set = new Set();
    let maxLength = 0;

    while (R < s.length) {

        while (set.has(s[R])) {
            set.delete(s[L]);
            L++;
        }

        set.add(s[R]);

        maxLength = Math.max(
            maxLength,
            R - L + 1
        );

        R++;
    }

    return maxLength;

---

## 🧠 Why the `while` Loop?

If the duplicate character isn't at `L`, removing only one character isn't enough.

Example:

    a b c b
    ↑     ↑
    L     R

`b` already exists in the window.

Remove `a`:

    b c b
    ↑   ↑
    L   R

`b` is still duplicated, so remove `b`:

    c b
    ↑ ↑
    L R

Now the window is valid again.

---

## Complexity

**Time:** `O(n)`

Although there is a nested `while` loop, both `L` and `R` only move forward.

- `R` moves at most `n` times.
- `L` moves at most `n` times.

Therefore the total work is `O(n)`.

**Space:** `O(n)`

The Set can contain up to `n` characters.

---

## ⭐ Key Takeaway

> When a window becomes invalid because of a duplicate, shrink it from the left until it becomes valid again. Don't reset the entire window.