# Valid Parentheses

## Problem

Given a string containing:

```text
( ) [ ] { }
```

determine whether the brackets are valid.

A string is valid when:

1. Every opening bracket has a corresponding closing bracket.
2. Brackets are closed in the correct order.
3. The most recently opened bracket is closed first.

### Examples

```text
()       → valid
()[]{}   → valid
([{}])   → valid

(]       → invalid
([)]     → invalid
((       → invalid
```

## Pattern

**Stack**

The key observation is that the most recently opened bracket must be closed first.

Example:

```text
([{}])
```

When we reach `{`:

```text
stack:
{
[
(
```

Therefore `}` must match `{`.

This is exactly **LIFO**.

## Approach

1. Create an empty Stack.
2. Iterate through the string.
3. If the character is an opening bracket, push it.
4. If it is a closing bracket:
   - Check the top of the Stack.
   - If it doesn't match, return `false`.
   - If it matches, pop it.
5. At the end, the Stack must be empty.

## Matching Brackets

```text
) → (
] → [
} → {
```

A `Map` makes this easier:

```js
const pairs = new Map([
    [")", "("],
    ["]", "["],
    ["}", "{"]
])
```

## Solution

```js
class Solution {
    /**
     * @param {string} s
     * @return {boolean}
     */
    isValid(s) {
        const stack = []

        const pairs = new Map([
            [")", "("],
            ["]", "["],
            ["}", "{"]
        ])

        for (const ch of s) {
            if (pairs.has(ch)) {
                if (stack.at(-1) !== pairs.get(ch)) {
                    return false
                }

                stack.pop()
            } else {
                stack.push(ch)
            }
        }

        return stack.length === 0
    }
}
```

## Walkthrough

For:

```text
"([{}])"
```

```text
( → push → ["("]

[ → push → ["(", "["]

{ → push → ["(", "[", "{"]

} → matches { → pop
  → ["(", "["]

] → matches [ → pop
  → ["("]

) → matches ( → pop
  → []
```

The Stack is empty at the end, so the string is valid.

## Important Edge Cases

### Unmatched opening brackets

```text
"((("
```

Stack isn't empty at the end → `false`.

### Closing bracket with no opening bracket

```text
")"
```

Nothing is available to match it → `false`.

### Wrong order

```text
"([)]"
```

When `)` appears:

```text
top = "["
expected = "("
```

They don't match → `false`.

This is why we check the **top of the Stack**, rather than simply checking whether a matching bracket exists somewhere in the Stack.

## Complexity

```text
Time:  O(n)
Space: O(n)
```

Each character is processed at most once.

## Interview Explanation
> I use a Stack because the most recently opened bracket must be the first one closed. I push opening brackets onto the Stack, and for every closing bracket I check whether it matches the top. If it doesn't match, the string is invalid. At the end, the Stack must be empty.