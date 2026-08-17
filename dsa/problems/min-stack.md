# Min Stack

## Core Idea

Use a **Stack + auxiliary state** to keep track of the minimum value at every level of the Stack.

Instead of storing:

```js
5
3
7
2
```

store:

```js
{
    val: 5,
    min: 5
}

{
    val: 3,
    min: 3
}

{
    val: 7,
    min: 3
}

{
    val: 2,
    min: 2
}
```

`min` stores the minimum value from that element down to the bottom of the Stack.

## Push

```js
const previousMin = stack.at(-1).min
const newMin = Math.min(previousMin, val)

stack.push({
    val: val,
    min: newMin
})
```

## Get Minimum

The current minimum is always stored at the top:

```js
stack.at(-1).min
```

Therefore:

```text
push()   → O(1)
pop()    → O(1)
top()    → O(1)
getMin() → O(1)
```

## Key Insight ⭐

> Store the **value + useful information about the Stack's state** together.

This avoids scanning the Stack to find the minimum.