# Sliding Window

## 💡 Core Idea

Maintain a **window** between two pointers, usually `L` and `R`, instead of repeatedly checking every possible subarray or substring.

- `R` expands the window.
- `L` shrinks the window when it becomes invalid.
- Both pointers only move forward.

Example:

    L →          ← R
    [ a  b  c  a ]

## When to Use

Look for problems involving:

- Subarrays or substrings
- Contiguous elements
- Longest/shortest valid window
- A condition that can be maintained while expanding or shrinking

## Basic Structure

    let L = 0;
    let R = 0;

    while (R < nums.length) {

        // Expand window

        while (/* window is invalid */) {
            // Shrink window
            L++;
        }

        // Process valid window

        R++;
    }

## Key Insight ⭐

Don't reset the window when it becomes invalid.

Instead, move `L` forward until the window becomes valid again.

Since both `L` and `R` only move forward, the total work can remain `O(n)`.

## Example

For:

    "abcabcbb"

Start with:

    a b c
    ↑   ↑
    L   R

When `R` reaches another `a`, the window becomes invalid:

    a b c a
    ↑     ↑
    L     R

Move `L` forward and remove elements until the duplicate is gone:

    b c a
      ↑ ↑
      L R

The window is valid again, so continue expanding `R`.

---

## 🎯 Required Frequency Window

Some Sliding Window problems require the window to contain specific elements with specific frequencies.

Use two frequency maps:

- `tCount` → required frequencies
- `windowCount` → frequencies inside the current window

Track:

- `need` → number of unique required characters
- `have` → number of required characters whose frequency is currently satisfied

Example:

    tCount:
    A → 2
    B → 1

    windowCount:
    A → 3
    B → 1

    have = 2
    need = 2

The window is valid because every required character has at least its required frequency.

### Maintaining `have`

When adding a character:

    if (windowCount.get(ch) === tCount.get(ch)) {
        have++;
    }

When removing a character:

    if (windowCount.get(ch) < tCount.get(ch)) {
        have--;
    }

Only decrement `have` when removing a character causes its frequency to fall **below** the required frequency.

### Minimum Window Variation

For longest-window problems:

> Expand until the window becomes invalid.

For minimum-window problems:

> Expand until the window becomes valid, then shrink from the left while it remains valid.

This allows us to find the smallest valid window.

---

## Common Data Structures

The data structure depends on the condition being maintained:

- `Set` → track unique elements
- `Map` → track frequencies or last positions
- Variables → maintain sums, counts, etc.

## Complexity

**Time:** Usually `O(n)`

Both pointers move only forward, so each element is processed a limited number of times.

**Space:** Depends on the data structure used by the window.