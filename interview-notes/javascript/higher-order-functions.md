# Higher-Order Functions

## Definition

A **Higher-Order Function (HOF)** is a function that:

- Takes one or more functions as arguments.
- Returns a function.
- Or does both.

### Example

A function that receives a callback:

    function calculate(a, b, operation) {
        return operation(a, b);
    }

`calculate` is a Higher-Order Function because it accepts `operation` as a function argument.

---

## Functions as First-Class Values

JavaScript treats functions like values.

A function can be:

- Stored in a variable.
- Passed as an argument.
- Returned from another function.

Example:

    const add = (a, b) => a + b;

    function calculate(operation) {
        return operation(5, 3);
    }

Here, `add` is passed around like a normal value.

---

## Callbacks

A **callback** is a function passed to another function to be executed during or after its execution.

    function processNumbers(numbers, callback) {
        return numbers.map(callback);
    }

Here:

- `processNumbers` is the Higher-Order Function.
- `callback` is the function being passed into it.

---

## `map()`

Use `map()` when you want to **transform every element**.

It returns a **new array**.

Example:

    const result = numbers.map((num) => num * 2);

Conceptually:

    [1, 2, 3]
        ↓ transform
    [2, 4, 6]

### Important

`map()` itself does not mutate the original array.

---

## `filter()`

Use `filter()` when you want to **keep only elements that satisfy a condition**.

It returns a **new array**.

Example:

    const result = numbers.filter((num) => num % 2 === 0);

Conceptually:

    [1, 2, 3, 4]
          ↓ keep even numbers
    [2, 4]

---

## `reduce()`

Use `reduce()` when you want to **combine elements into an accumulated result**.

It returns one accumulated result.

Example:

    const sum = numbers.reduce(
        (acc, num) => acc + num,
        0
    );

### Accumulator

The accumulator carries the result from one iteration to the next.

    0 → 1 → 3 → 6 → 10

Final result:

    10

The returned result does not have to be a primitive. `reduce()` can also produce an array or object.

---

## `forEach()`

Use `forEach()` when you want to perform an action for each element, usually for a **side effect**.

It returns:

    undefined

Example:

    numbers.forEach((num) => {
        console.log(num);
    });

Unlike `map()`, `forEach()` does not create a new array from the callback's return values.

---

## `map()` vs `forEach()`

| `map()` | `forEach()` |
|---|---|
| Returns a new array | Returns `undefined` |
| Used for transformation | Usually used for side effects |
| Can be chained | Cannot be chained based on its return value |

Example: method chaining

    numbers
        .map(...)
        .filter(...)

works because `map()` returns an array.

---

## Method Chaining

Higher-order array methods can be chained when the previous method returns a compatible value.

Example:

    const result = numbers
        .filter((num) => num % 2 === 0)
        .map((num) => num * 10);

Execution:

    [1, 2, 3, 4]
          ↓ filter
    [2, 4]
          ↓ map
    [20, 40]

---

## Returning Functions

A Higher-Order Function can also return another function.

Example:

    function multiplier(factor) {
        return function (number) {
            return number * factor;
        };
    }

Usage:

    const double = multiplier(2);

    double(5);

Result:

    10

---

## Higher-Order Functions + Closures

When a function returns another function, the returned function can preserve access to variables from its outer scope.

    function multiplier(factor) {
        return function (number) {
            return number * factor;
        };
    }

The returned function closes over `factor`.

> The returned function together with its preserved access to the outer variable forms the closure.

---

## Implementing `map()`

The core idea behind `map()` can be reproduced using a loop:

    function myMap(numbers, callback) {
        const result = [];

        for (let i = 0; i < numbers.length; i++) {
            result.push(callback(numbers[i]));
        }

        return result;
    }

### Core behavior

1. Create a new array.
2. Iterate through the original array.
3. Call the callback for each element.
4. Store the callback's return value.
5. Return the new array.

---

## Interview Recognition

When asked whether a function is a Higher-Order Function, ask:

> **Does this function receive a function, return a function, or both?**

If yes, it is a Higher-Order Function.

---

## Common Interview Distinctions

### `map()`

> Transform every element and return a new array.

### `filter()`

> Keep elements that satisfy a condition and return a new array.

### `reduce()`

> Combine elements into an accumulated result.

### `forEach()`

> Execute something for each element, usually for side effects.

---

## Interview Takeaways

- Functions in JavaScript are **first-class values**.
- A Higher-Order Function can **accept functions or return functions**.
- A callback is a function passed to another function.
- `map()` transforms.
- `filter()` selects.
- `reduce()` accumulates.
- `forEach()` performs side effects.
- Returning a function can create a **closure** over variables from the outer scope.