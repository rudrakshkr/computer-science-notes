# Daily Temperatures — LeetCode 739

## 💡 Core Idea

For each temperature, find how many days until a **warmer temperature** appears.

Use a **Monotonic Decreasing Stack**.

The Stack stores the **indices of temperatures still waiting for a warmer day**.

---

## Approach

For every index `i`:

1. While the Stack is not empty and the current temperature is greater than the temperature at the Stack's top:
   - Pop the previous index.
   - The current day is its next warmer day.
   - Store `i - prev` as the answer.
2. Push the current index into the Stack.

```js
const answer = new Array(temperatures.length).fill(0)
const stack = []

for (let i = 0; i < temperatures.length; i++) {
    while (
        stack.length > 0 &&
        temperatures[i] > temperatures[stack.at(-1)]
    ) {
        const prev = stack.pop()
        answer[prev] = i - prev
    }

    stack.push(i)
}

return answer
```

---

## Why `while`?

One temperature can be the answer for **multiple previous temperatures**.

Example:

```text
[75, 71, 69, 72]
```

When `72` arrives:

```text
72 > 69 → pop
72 > 71 → pop
72 > 75 → stop
```

After each `pop()`, the next element becomes the new top of the Stack, so we keep checking.

---

## Why Store Indices?

We need to calculate the number of days between temperatures:

```js
answer[prev] = i - prev
```

Therefore, the Stack stores **indices**, not temperature values.

---

## Complexity

```text
Time:  O(n)
Space: O(n)
```

Each index is pushed once and popped at most once, so the total time is `O(n)` despite the nested `while` loop.

---

## Pattern to Remember ⭐

> **Next greater element → consider a Monotonic Stack.**

For this problem:

```text
Current temperature
        ↓
Is it greater than Stack top?
        ↓
       YES
        ↓
      pop
        ↓
Calculate distance
        ↓
Check the new Stack top
``` 