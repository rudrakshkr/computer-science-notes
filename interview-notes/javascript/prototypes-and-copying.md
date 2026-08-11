# Prototypes & Prototype Chain

## 💡 What is a Prototype?

A **prototype is an object that another object can inherit properties and methods from**.

Every JavaScript object has an internal `[[Prototype]]` link that points to another object or `null`.

---

## 🔗 Prototype Chain

When JavaScript looks for a property:

1. Check the object itself.
2. If not found, check its `[[Prototype]]`.
3. Continue up the chain.
4. If the property is never found, return `undefined`.

```text
object
   ↓
prototype
   ↓
Object.prototype
   ↓
null
```

This sequence is called the **Prototype Chain**.

---

## `prototype` vs `__proto__`

### `prototype`

`prototype` is a property commonly found on **constructor functions**.

It becomes the prototype of objects created using `new`.

```js
function Person() {}

const person = new Person();

person.__proto__ === Person.prototype;
// true
```

---

### `__proto__`

`__proto__` refers to the object's actual `[[Prototype]]`.

```js
person.__proto__
```

Modern JavaScript prefers:

```js
Object.getPrototypeOf(person)
```

---

## `Object.prototype`

`Object.prototype` is the prototype at the top of the normal prototype chain for most ordinary objects.

```text
person
   ↓
Person.prototype
   ↓
Object.prototype
   ↓
null
```

Not every object inherits from it. For example:

```js
Object.create(null)
```

creates an object with no prototype.

---

## 🔎 Property Lookup

```js
const person = {
    name: "A"
};

person.toString;
```

`person` doesn't contain `toString`, so JavaScript searches:

```text
person
   ↓
Object.prototype
   ↓
toString ✅
```

The inherited method is returned.

---

## `new`

When using:

```js
const p1 = new Person("A");
```

JavaScript essentially:

1. Creates a new object.
2. Links its `[[Prototype]]` to `Person.prototype`.
3. Calls `Person` with the new object as `this`.
4. Returns the new object.

```text
new Person("A")
       ↓
create object
       ↓
[[Prototype]] → Person.prototype
       ↓
Person.call(newObject, "A")
       ↓
return object
```

---

## 🧠 Prototype Methods Are Shared

### Method inside constructor

```js
function Person() {
    this.greet = function () {};
}
```

Every instance gets its **own function**.

```text
p1 → greet
p2 → greet
p3 → greet
```

---

### Method on prototype

```js
Person.prototype.greet = function () {};
```

All instances share the same function.

```text
p1 ──┐
p2 ──┼──→ Person.prototype → greet
p3 ──┘
```

This avoids creating a separate function for every instance.

---

## `hasOwnProperty()` vs `in`

### `hasOwnProperty()`

Checks only properties directly owned by the object.

```js
p1.hasOwnProperty("greet");
```

Returns `false` if `greet` only exists on the prototype.

---

### `in`

Checks the object **and the entire prototype chain**.

```js
"greet" in p1;
```

Can return `true` even when `greet` is inherited.

```text
hasOwnProperty()
→ own properties only

in
→ own + inherited properties
```

---

## 👤 Property Shadowing

If an object has its own property with the same name as an inherited property, the own property is used.

```js
Person.prototype.greet = function () {
    return "Hello";
};

p1.greet = function () {
    return "Hi";
};
```

```text
p1
 └── greet → "Hi" ✅
       ↓
Person.prototype
 └── greet → "Hello"
```

JavaScript stops searching once it finds `greet` on `p1`.

---

## `Object.create()`

```js
const animal = {
    eat() {
        return "eating";
    }
};

const dog = Object.create(animal);
```

Creates:

```text
dog
 ↓
animal
 ↓
Object.prototype
 ↓
null
```

So:

```js
dog.eat();
```

works even though `eat` isn't directly inside `dog`.

---

# Prototype Pollution

## 💡 Definition

**Prototype pollution is a security vulnerability where an attacker modifies a shared prototype**, such as `Object.prototype`, causing unintended properties or behavior to be inherited by other objects.

Example:

```js
Object.prototype.isAdmin = true;
```

Now:

```js
const user = {};

console.log(user.isAdmin);
// true
```

The property wasn't added directly to `user`.

It was inherited from the polluted prototype.

---

## ⚠ Why Is It Dangerous?

`Object.prototype` is shared by many ordinary objects.

Therefore:

```text
Object.prototype
       ↓
polluted property
       ↓
many unrelated objects
```

can be affected.

---

# Shallow vs Deep Copy

## 💡 Shallow Copy

A shallow copy creates a new **top-level object**, but nested objects and arrays still share references.

```text
original ──→ nested object
copy     ────┘
```

Common methods:

```js
const copy = { ...obj }; -> Spread Operator

const copy = Object.assign({}, obj);

const arrayCopy = [...arr];

const arrayCopy = arr.slice();
```

---

## 💡 Deep Copy

A deep copy recursively creates independent nested structures.

```text
original ──→ nested object A
copy     ──→ nested object B
```

Changes to nested properties do not affect the original.

---

## `structuredClone()`

`structuredClone()` provides a built-in way to deep-clone many supported JavaScript data types.

```js
const copy = structuredClone(original);
```

It is generally preferable to the old JSON workaround when the data is supported.

---

## `JSON.parse(JSON.stringify())`

An older workaround for cloning simple JSON-compatible data:

```js
const copy = JSON.parse(JSON.stringify(original));
```

However, it has significant limitations and does not preserve things such as:

- Functions
- `undefined`
- Certain built-in types
- Some special values

So it should not be treated as a general-purpose deep-cloning solution.