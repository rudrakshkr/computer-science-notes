# Minimum Window Substring

## 🔴 Problem

Given two strings `s` and `t`, return the **smallest substring of `s`** that contains every character in `t`, including duplicate characters.

Example:

    Input:
    s = "ADOBECODEBANC"
    t = "ABC"

    Output:
    "BANC"

---

## Pattern

**Sliding Window + Frequency Map**

---

## 💡 Core Idea

Create two frequency maps:

- `tCount` → frequencies required by `t`
- `windowCount` → required characters currently inside the window

Use:

- `R` to expand the window
- `L` to shrink the window
- `need` → number of unique characters that must be satisfied
- `have` → number currently satisfying their required frequency

When:

    have === need

the current window contains everything required by `t`.

Then shrink from `L` while the window remains valid to find the smallest possible window.

---

## Algorithm

    1. Build the frequency map for t -> tCount and s -> windowCount.

    2. Set:
       L = 0
       R = 0
       have = 0
       need = tCount.size

    3. Expand R through s.

    4. If s[R] is required:
       - Increase its frequency in windowCount.
       - If its frequency reaches the required amount:
         have++

    5. When have === need:
       - Record the window if it's the smallest so far.
       - Remove s[L] from the window.
       - If its frequency falls below the required amount:
         have--
       - Move L forward.

    6. Continue until R reaches the end.

---

## 🧠 Important `have` Logic

Suppose:

    tCount:
    A → 2

If:

    windowCount:
    A → 1

`A` is not satisfied yet.

When it becomes:

    A → 2

we do:

    have++

If the window later contains:

    A → 3

`A` is still only counted once in `have`.

When shrinking:

    A → 3
    A → 2   // still satisfied
    A → 1   // no longer satisfied → have--

So `have` tracks **satisfied character types**, not the total number of matching characters.

---

## Why Shrink While Valid?

Once:

    have === need

we already have a valid window.

But it may contain unnecessary characters.

Example:

    ADOBEC

contains `A`, `B`, and `C`, but there may be a smaller valid window later.

Therefore:

> Expand `R` until the window is valid, then move `L` forward while it remains valid.

---

## Complexity

**Time:** `O(n + m)`

- Building `tCount` takes `O(m)`.
- `R` moves through `s` once.
- `L` also only moves forward through `s`.

Even with the nested `while`, both pointers move at most `n` times.

**Space:** `O(m)`

Frequency maps store the characters required by `t`.

---

## ⭐ Key Takeaway

> For minimum-window problems, expand until the window satisfies all requirements, then shrink it as much as possible while it remains valid.

Use frequency maps when the problem requires not just the presence of characters, but specific **frequencies** of those characters.