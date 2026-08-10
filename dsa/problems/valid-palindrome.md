# Valid Palindrome

## Pattern
Two Pointers

## Approach

- Place one pointer at the beginning and one at the end.
- Move both pointers toward the center.
- Skip non-alphanumeric characters.
- Compare characters case-insensitively.
- If any pair doesn't match → `false`.
- If the pointers meet without a mismatch → `true`.

## Key Insight ⭐

Don't create a cleaned copy of the string.

The pointers can skip invalid characters while traversing the original string, giving **O(1) extra space**.

## Complexity

Time: O(n)

Space: O(1)

## Interview Takeaway

> When comparing characters from opposite ends of a string, Two Pointers can avoid creating another string and solve the problem in linear time with constant extra space.