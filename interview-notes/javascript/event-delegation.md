# Event Delegation

## Definition

**Event Delegation** is a technique where a single event listener is attached to a **parent element** instead of adding separate listeners to each child element.

It relies on **event bubbling**.

---

## Event Bubbling

When an event occurs on an element, it can propagate upward through its ancestors.

```text
Child
  ↑
Parent
  ↑
Grandparent
  ↑
Document
```

Example:

```html
<ul id="users">
    <li>User 1</li>
    <li>User 2</li>
    <li>User 3</li>
</ul>
```

A click on:

```text
<li>User 2</li>
```

can bubble up to:

```text
<ul id="users">
```

---

## Event Delegation Pattern

Instead of:

```text
li 1 → listener
li 2 → listener
li 3 → listener
```

Use:

```text
ul → one listener
      ↓
   determine clicked child
```

Example:

```js
users.addEventListener("click", (event) => {
    const li = event.target.closest("li");

    if (li) {
        console.log(li.textContent);
    }
});
```

---

## `event.target`

`event.target` refers to the **actual element where the event originated**.

If the user clicks:

```html
<li>User 2</li>
```

then:

```js
event.target
```

refers to the `<li>`.

If the structure is:

```html
<li>
    <button>
        <span>User 2</span>
    </button>
</li>
```

and the user clicks the `<span>`:

```text
event.target → <span>
```

The target can therefore be a nested element.

---

## `event.currentTarget`

`event.currentTarget` refers to the **element on which the event listener is registered**.

Example:

```js
users.addEventListener("click", (event) => {
    console.log(event.target);
    console.log(event.currentTarget);
});
```

If the `<li>` is clicked:

```text
event.target
→ <li>

event.currentTarget
→ <ul id="users">
```

### Interview Distinction

> `target` = where the event originated.

> `currentTarget` = where the listener is attached.

---

## `closest()`

`closest()` finds the nearest ancestor that matches a selector.

Example:

```js
const li = event.target.closest("li");
```

If the click starts on a nested element:

```text
<span>
  ↑
<button>
  ↑
<li>
```

then:

```js
event.target.closest("li")
```

returns the `<li>`.

### Important

`closest()` **walks upward** through ancestors.

---

## `matches()`

`matches()` checks whether the **current element itself** matches a selector.

Example:

```js
event.target.matches("button")
```

If `event.target` is a `<button>`, it returns `true`.

If `event.target` is a `<span>` inside the button, it returns `false`.

### Difference

```text
matches()
→ checks the current element only

closest()
→ checks the current element and then walks upward
```

---

## Dynamic Elements

Event Delegation is especially useful for dynamically created elements.

Without delegation:

```js
document.querySelectorAll("li").forEach((li) => {
    li.addEventListener("click", handleClick);
});
```

If a new `<li>` is added later, it does **not automatically have the listener**.

With delegation:

```js
users.addEventListener("click", (event) => {
    const li = event.target.closest("li");

    if (li) {
        handleClick(li);
    }
});
```

The parent listener already exists, so newly added children can also trigger it through event bubbling.

---

## `stopPropagation()`

Stops an event from propagating further through the DOM.

```js
event.stopPropagation();
```

It controls **event propagation**, not the browser's default behavior.

---

## `preventDefault()`

Prevents the browser's default action for an event.

Example:

```js
form.addEventListener("submit", (event) => {
    event.preventDefault();
});
```

This prevents the browser's normal form submission behavior.

Another example:

```js
link.addEventListener("click", (event) => {
    event.preventDefault();
});
```

This prevents normal link navigation.

### Difference

```text
stopPropagation()
→ stops event propagation

preventDefault()
→ stops the browser's default action
```

---

## Defensive `closest()` Check

`closest()` can return `null` when no matching ancestor exists.

Example:

```js
const li = event.target.closest("li");

if (li) {
    console.log(li.textContent);
}
```

Without the check, attempting to access:

```js
li.textContent
```

when `li` is `null` causes a **TypeError**.

---

## `contains()`

`contains()` checks whether an element is inside another element.

Example:

```js
users.contains(button)
```

returns:

```text
true  → button is inside users
false → button is outside users
```

A defensive delegated-event check can be:

```js
const button = event.target.closest("button");

if (!button || !users.contains(button)) {
    return;
}
```

This ensures that the matched button actually belongs to the intended parent.

---

## Why Use Event Delegation?

- Fewer event listeners.
- Useful for large collections of elements.
- Works well with dynamically added elements.
- Centralizes event handling.
- Uses event bubbling to identify the originating child.

---

## Interview Recognition

Ask:

> **Do multiple child elements need the same event behavior, especially when some children may be added dynamically?**

If yes, consider **Event Delegation**.

---

## Interview Takeaways

- Event Delegation uses **one parent listener** to handle events from multiple children.
- It relies on **event bubbling**.
- `event.target` is the element where the event originated.
- `event.currentTarget` is the element where the listener is attached.
- `closest()` walks upward to find a matching ancestor.
- `matches()` checks only the current element.
- Delegation works naturally with dynamically added children.
- `stopPropagation()` controls propagation.
- `preventDefault()` controls the browser's default action.
- `contains()` can provide an additional defensive boundary check.