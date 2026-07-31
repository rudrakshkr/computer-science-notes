# Hoisting & Scope

## What is Hoisting?

Hoisting is JavaScript's behavior of creating variable and function bindings during the Creation Phase before code execution begins.

---

## Execution Phases

### 1. Creation Phase

- Memory is allocated for variables and functions.
- `var` is initialized with `undefined`.
- `let` and `const` remain uninitialized (TDZ).
- Function declarations are fully available.

### 2. Execution Phase

- Code executes line by line.
- Assignments happen.
- Functions are called.

---

## var

- Hoisted
- Initialized with `undefined`

```js
console.log(a);
var a = 10;
```

**Output**

```text
undefined
```

---

## let & const

- Hoisted
- Stay in the Temporal Dead Zone until their declaration executes

```js
console.log(a);
let a = 10;
```

**Output**

```text
ReferenceError
```

---

## Temporal Dead Zone (TDZ)

The TDZ is the time between entering a scope and executing the variable declaration.

Accessing a `let` or `const` variable during this period throws a `ReferenceError`.

---

## Scope Lookup

JavaScript searches for variables in this order:

1. Current Scope
2. Parent Scope
3. Global Scope

If the variable is not found, a `ReferenceError` is thrown.

---

## Variable Shadowing

A local variable with the same name hides the outer variable.

```js
let a = 10;

function test() {
    let a = 20;
    console.log(a);
}

test();
```

**Output**

```text
20
```

---

## TDZ + Shadowing

```js
let a = 10;

function test() {
    console.log(a);
    let a = 20;
}

test();
```

**Output**

```text
ReferenceError
```

The local variable exists but is inside the TDZ, so JavaScript does not search the global scope. The variable `a` is already inside the function local scope, so it will not look at the global scope.

---

## Function Scope with `var`

```js
var a = 10;

function test() {
    console.log(a);
    var a = 20;
    console.log(a);
}

test();

console.log(a);
```

**Output**

```text
undefined
20
10
```

---

## Function Declaration vs Function Expression

### Function Declaration

```js
hello();

function hello() {
    console.log("Hello");
}
```

**Output**

```text
Hello
```

### Function Expression

```js
hello();

var hello = function () {
    console.log("Hello");
};
```

**Output**

```text
TypeError
```

---

## Quick Revision

- `var` → Hoisted, initialized as `undefined`
- `let` & `const` → Hoisted, remain in the TDZ
- JavaScript searches the current scope first
- Local variables shadow outer variables
- A variable in the TDZ still shadows outer variables
- Function declarations are fully hoisted