# `this` Keyword

## 💡 What is `this`?

`this` is a special keyword that refers to an object.

Its value is determined by **how a function is called (call site)**, **not where the function is defined**.

---

## Object Method

```javascript
const person = {
    name: "Rudraksh",

    greet() {
        console.log(this.name);
    },
};

person.greet();
```

Output

```
Rudraksh
```

### Why?

`greet()` is called as a method of `person`.

```
person.greet()

↓

this === person
```

---

## Standalone Function

```javascript
const person = {
    name: "Rudraksh",

    greet() {
        console.log(this.name);
    },
};

const greet = person.greet;

greet();
```

Output (Strict Mode)

```
TypeError
```

Output (Non-Strict Mode)

```
undefined
```

### Why?

The function is no longer called as:

```
person.greet()
```

Instead:

```
greet()
```

No owning object exists.

```
this === undefined in Strict Mode
this === window object in Non Strict Mode
```

---

## bind()

Returns a **new function** with `this` permanently bound to it.

```javascript
const bound = person.greet.bind(person);

bound();
```

Output

```
Rudraksh
```

---

## call()

Calls the function **immediately**. Does not return a new function.

```javascript
greet.call(person);
```

---

## apply()

Calls the function **immediately**. Does not create a new function.

Arguments are passed as an array.

```javascript
greet.apply(person, ["Delhi", "India"]);
```

---

## bind() vs call() vs apply()

| Method | Calls Immediately | Returns New Function |
|---------|------------------:|---------------------:|
| `call()` | ✅ | ❌ |
| `apply()` | ✅ | ❌ |
| `bind()` | ❌ | ✅ |

---

## Regular Function

```javascript
const person = {
    name: "Rudraksh",

    greet() {
        console.log(this.name);
    },
};

person.greet();
```

`this` is determined by the **call site**.

---

## Arrow Function

```javascript
const person = {
    name: "Rudraksh",

    greet: () => {
        console.log(this.name);
    },
};
```

Arrow functions **do not have their own `this`.**

They inherit `this` from the **surrounding lexical scope**.

---

## Regular vs Arrow

| Regular Function | Arrow Function |
|------------------|----------------|
| Has its own `this` | Doesn't have its own `this` |
| `this` depends on call site | `this` depends on lexical scope |

---

## Interview Tips

✅ `this` depends on **how a function is called**.

✅ Arrow functions inherit `this` from their surrounding scope.

✅ `bind()` returns a **new function**.

✅ `call()` and `apply()` invoke the function immediately.