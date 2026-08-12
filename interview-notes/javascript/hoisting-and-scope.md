# Hoisting & Scope

## What is Hoisting?

Hoisting is JavaScript's behavior of creating variable and function bindings during the Creation Phase before code execution begins.

---

## ⚙️ Execution Context

An **Execution Context** is the environment in which JavaScript evaluates and executes code.

### Types

- **Global Execution Context** → created when the program starts.
- **Function Execution Context** → created whenever a function is called.

A function execution context contains information such as:

- Variables and parameters
- Lexical environment
- Scope information
- `this`

---

## 📚 Execution Context & Call Stack

The **Call Stack** keeps track of currently executing execution contexts.

```js
function greet() {
    console.log("Hello");
}

greet();
```

Conceptually:

```text
Global Execution Context
        ↓
    greet() context
        ↓
    execute greet()
        ↓
    pop greet()
```

When a function is called, its execution context is pushed onto the call stack. When it finishes, it is popped.

---

## 🌐 Lexical Environment & Scope Chain

A **Lexical Environment** is the environment where JavaScript stores and resolves variables based on where the code is written.

JavaScript searches for variables from the current scope outward:

```text
Current Scope
      ↓
Parent Scope
      ↓
Global Scope
```

Example:

```js
let x = 10;

function outer() {
    let y = 20;

    function inner() {
        console.log(x);
        console.log(y);
    }

    inner();
}
```

`inner()` can access `y` from `outer` and `x` from the global scope.

> JavaScript stops searching as soon as it finds the variable.

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

The local variable exists but is inside the TDZ, so JavaScript does not search the global scope. The local `a` already shadows the global `a`.

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

Function declarations are fully hoisted.

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

The variable `hello` is hoisted as `undefined`, but the function assignment happens only during execution.

---

## 🎯 Variable Shadowing Example

```js
let x = 10;

function outer() {
    let x = 20;

    function inner() {
        console.log(x);
    }

    inner();
}

outer();
```

**Output**

```text
20
```

Lookup:

```text
inner
  ↓
x ❌
  ↓
outer
  ↓
x = 20 ✅
```

The `x` inside `outer` shadows the global `x`.

---

## 🔗 Execution Context + Closures

A function can retain access to variables from its outer lexical environment even after the outer function has finished executing.

```js
function outer() {
    let count = 0;

    return function inner() {
        count++;
        return count;
    };
}

const counter = outer();

counter(); // 1
counter(); // 2
counter(); // 3
```

`outer()` finishes and its execution context is removed from the call stack, but `inner()` still has access to `count` through its closure.

See **closures.md** for the complete closure notes.

---

## ⭐ Quick Revision

- **Execution Context** → environment where JavaScript executes code.
- **Global Execution Context** → created when the program starts.
- **Function Execution Context** → created when a function is called.
- **Call Stack** → manages active execution contexts.
- **Lexical Environment** → used to resolve variables according to their lexical scope.
- **Scope Chain** → current scope → parent scope → global scope.
- `var` → Hoisted, initialized as `undefined`
- `let` & `const` → Hoisted, remain in the TDZ
- Local variables shadow outer variables.
- A variable in the TDZ still shadows outer variables.
- Function declarations are fully hoisted.
- **Creation Phase** → execution context is set up.
- **Execution Phase** → code actually executes.