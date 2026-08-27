# DOM / Browser Concepts

## 1. DOM — Document Object Model

The **DOM (Document Object Model)** is the browser's in-memory representation of an HTML document.

The browser parses HTML and represents it as a **tree of objects/nodes** that JavaScript can interact with.

Conceptually:

    Document
      └── html
           ├── head
           └── body
                ├── div
                └── button

JavaScript can use the DOM to:

- Find elements
- Read content
- Change content
- Change styles/classes
- Create elements
- Remove elements
- Attach event listeners

### Common DOM APIs

    document.querySelector()
    document.querySelectorAll()
    document.getElementById()

    element.textContent
    element.innerHTML
    element.innerText

    element.classList
    element.append()
    element.remove()

---

## 2. `querySelector()`

Returns the **first element** matching a CSS selector.

Example:

    const button = document.querySelector(".button");

If no matching element exists:

    null

---

## 3. `querySelectorAll()`

Returns **all matching elements** as a `NodeList`.

Example:

    const items = document.querySelectorAll(".item");

Unlike `querySelector()`, it returns multiple matching elements.

If nothing matches, the result is an empty `NodeList`.

### Interview Distinction

    querySelector()
    → first matching element

    querySelectorAll()
    → all matching elements

---

## 4. `textContent` vs `innerHTML` vs `innerText`

These are frequently confused in interviews.

### `textContent`

Reads or sets the text content of an element.

    element.textContent = "Hello";

It treats the assigned value as text rather than HTML.

Example:

    element.textContent = "<strong>Hello</strong>";

The browser displays the characters rather than creating a `<strong>` element.

### `innerHTML`

Reads or sets the HTML inside an element.

    element.innerHTML = "<strong>Hello</strong>";

The browser parses the string as HTML.

### Security Concern

Never insert untrusted user-controlled HTML directly using `innerHTML` without appropriate sanitization.

Otherwise, malicious input can lead to **Cross-Site Scripting (XSS)**.

### `innerText`

Represents the text as rendered/visible by the browser.

It is affected by CSS and layout.

### Interview Comparison

    textContent
    → text in the DOM

    innerHTML
    → HTML markup inside the element

    innerText
    → rendered/visible text

---

## 5. DOM Manipulation

JavaScript can create new elements:

    const div = document.createElement("div");

Add an element:

    parent.append(div);

Remove an element:

    div.remove();

Modify classes:

    div.classList.add("active");
    div.classList.remove("active");
    div.classList.toggle("active");

The DOM is therefore mutable through JavaScript.

---

## 6. Event Propagation

When an event occurs, it propagates through the DOM.

The simplified event flow is:

    Capturing
        ↓
      Target
        ↓
     Bubbling

### Capturing Phase

The event travels **from the outer ancestors toward the target**.

Conceptually:

    document
       ↓
     body
       ↓
      div
       ↓
     button

### Target Phase

The event reaches the element that originally triggered it.

### Bubbling Phase

The event travels **from the target back upward through its ancestors**.

Conceptually:

    button
       ↑
      div
       ↑
     body
       ↑
   document

Event Delegation relies heavily on **event bubbling**.

---

## 7. `event.target`

`event.target` is the **actual element where the event originated**.

Example:

    <ul>
        <li>User 1</li>
        <li>User 2</li>
    </ul>

If the user clicks `User 2`:

    event.target
    → <li>User 2</li>

If there is a nested element:

    <li>
        <button>
            <span>User 2</span>
        </button>
    </li>

and the user clicks the `<span>`:

    event.target
    → <span>

The target can therefore be a deeply nested element.

---

## 8. `event.currentTarget`

`event.currentTarget` is the element on which the current event listener is registered.

Example:

    users.addEventListener("click", (event) => {
        console.log(event.target);
        console.log(event.currentTarget);
    });

If the `<li>` is clicked:

    event.target
    → <li>

    event.currentTarget
    → <ul>

### Interview Distinction

    target
    → where the event originated

    currentTarget
    → where the listener is attached

This is especially important in Event Delegation.

---

## 9. `addEventListener()`

Used to attach an event listener to an element.

Example:

    button.addEventListener("click", handleClick);

The callback runs when the specified event occurs.

### Removing a Listener

    button.removeEventListener("click", handleClick);

The same function reference should generally be used when removing the listener.

For example:

    function handleClick() {
        console.log("clicked");
    }

    button.addEventListener("click", handleClick);
    button.removeEventListener("click", handleClick);

Using separate anonymous function objects does not give `removeEventListener()` the same reference.

---

## 10. `preventDefault()`

Prevents the browser's **default action** for an event.

Example:

    form.addEventListener("submit", (event) => {
        event.preventDefault();
    });

This prevents the browser from performing its normal form submission behavior.

For a link:

    link.addEventListener("click", (event) => {
        event.preventDefault();
    });

This prevents normal navigation.

### Important

    preventDefault()
    → prevents browser default behavior

It does **not** stop event propagation.

---

## 11. `stopPropagation()`

Stops an event from continuing through its propagation path.

Example:

    button.addEventListener("click", (event) => {
        event.stopPropagation();
    });

This can prevent the event from continuing to parent listeners during propagation.

### Important

    stopPropagation()
    → controls event propagation

    preventDefault()
    → controls browser default behavior

They solve different problems.

---

## 12. Event Delegation

**Event Delegation** means attaching one listener to a parent and handling events from multiple child elements.

Instead of:

    button 1 → listener
    button 2 → listener
    button 3 → listener

use:

    parent → one listener

The parent identifies which child triggered the event.

This works because of **event bubbling**.

### Benefits

- Fewer event listeners
- Centralized event handling
- Useful for large collections of elements
- Works naturally with dynamically added children

---

## 16. Reflow / Layout

**Reflow**, commonly discussed as **layout**, occurs when the browser needs to recalculate the geometry and positioning of elements.

Examples of changes that may require layout recalculation:

- Changing width
- Changing height
- Changing position
- Adding or removing elements
- Changing properties that affect layout

Layout calculations can be relatively expensive.

---

## 17. Repaint

A **repaint** occurs when the browser needs to redraw visual aspects of an element without necessarily recalculating its layout.

Examples can include changes such as:

- Text color
- Background color
- Some visual properties

General rule:

    Reflow / layout
    → generally more expensive

    Repaint
    → generally cheaper

The exact cost depends on the property and browser implementation.

---

## 18. Layout Thrashing

**Layout thrashing** occurs when code repeatedly alternates between layout reads and layout-changing writes.

Example pattern:

    read layout
    write layout
    read layout
    write layout
    read layout
    write layout

Certain layout reads can force the browser to calculate updated layout.

Common layout-related reads include:

    element.offsetHeight
    element.offsetWidth
    element.getBoundingClientRect()

### Better Approach

Group reads together and writes together where practical.

Instead of repeatedly alternating:

    read
    write
    read
    write

prefer:

    read
    read
    read

    write
    write
    write

This can reduce unnecessary layout calculations.

---

## 19. Browser Rendering Pipeline

A simplified rendering pipeline is:

    HTML + CSS
        ↓
    DOM + CSSOM
        ↓
    Render Tree
        ↓
    Layout
        ↓
    Paint
        ↓
    Composite

### Simplified Meaning

**DOM + CSSOM**

The browser constructs structures representing the document and styles.

**Render Tree**

Determines what should actually participate in rendering.

**Layout**

Calculates dimensions and positions.

**Paint**

Draws visual elements.

**Composite**

Combines rendered layers for final display.

---

## 20. `requestAnimationFrame()`

`requestAnimationFrame()` schedules a callback to run before the browser's next repaint.

Example:

    requestAnimationFrame(() => {
        // visual update
    });

It is useful for:

- Animations
- Smooth visual updates
- Repeated UI movement

It synchronizes updates with the browser's rendering cycle more appropriately than arbitrary high-frequency timers for visual animation.

---

## 21. JavaScript and the Browser

JavaScript execution itself is generally **single-threaded** in the browser's main JavaScript environment.

The browser also provides APIs for tasks such as:

- Timers
- Network requests
- DOM events
- Rendering-related operations

The event loop coordinates asynchronous callbacks and JavaScript execution.

High-level model:

    JavaScript
        ↓
    Browser APIs
        ↓
    Task / microtask queues
        ↓
    Event Loop
        ↓
    Call Stack

---

## 22. `localStorage`

`localStorage` provides browser-side key-value storage.

Important properties:

- Data persists across browser sessions.
- Values are stored as strings.
- JavaScript can access it directly.

Example:

    localStorage.setItem("theme", "dark");

    const theme = localStorage.getItem("theme");

    localStorage.removeItem("theme");

### Important Security Consideration

Because JavaScript can access `localStorage`, sensitive authentication information stored there can be exposed if an attacker successfully executes JavaScript through an XSS vulnerability.

---

## 23. `sessionStorage`

`sessionStorage` is similar to `localStorage`, but its data is associated with the current browser tab/session.

Example:

    sessionStorage.setItem("name", "Rudraksh");

    const name = sessionStorage.getItem("name");

General distinction:

    localStorage
    → persists across browser sessions

    sessionStorage
    → associated with the current tab/session

Both store values as strings.

---

## 24. Cookies

Cookies are small pieces of data associated with a domain.

Unlike `localStorage` and `sessionStorage`, cookies can be automatically included with HTTP requests.

Cookies can also use security-related attributes such as:

    HttpOnly
    Secure
    SameSite

### Important

`HttpOnly` cookies cannot be read directly by client-side JavaScript.

This can reduce exposure of certain authentication cookies to JavaScript-based XSS attacks.

---

## 25. `localStorage` vs `sessionStorage` vs Cookies

    localStorage
    → browser storage
    → persists across sessions
    → JavaScript accessible

    sessionStorage
    → browser storage
    → generally limited to the current tab/session
    → JavaScript accessible

    Cookies
    → associated with a domain
    → can be sent automatically with requests
    → can use HttpOnly, Secure and SameSite

### Interview Consideration

Do not blindly store sensitive authentication data in `localStorage`.

Understand the security model and the consequences of XSS.

---

## 26. `async` vs `defer`

These attributes affect external scripts.

### `defer`

A deferred script:

- Downloads while HTML is being parsed.
- Executes after HTML parsing finishes.
- Preserves execution order between deferred scripts.

Conceptually:

    Parse HTML
       ↓
    Download script in parallel
       ↓
    Finish parsing
       ↓
    Execute script

### `async`

An asynchronous script:

- Downloads while HTML is being parsed.
- Executes as soon as it finishes downloading.
- Does not guarantee execution order.

Conceptually:

    Parse HTML
       ↓
    Script finishes downloading
       ↓
    Execute immediately

### Interview Difference

    defer
    → execute after parsing
    → order preserved

    async
    → execute as soon as ready
    → order not guaranteed

---

## 27. `DOMContentLoaded` vs `load`

### `DOMContentLoaded`

Fires when the HTML document has been fully parsed and the DOM is ready.

It does not wait for every external resource to finish loading.

### `load`

Fires after the page and its dependent resources have finished loading.

### Difference

    DOMContentLoaded
    → DOM is ready

    load
    → page/resources are loaded

---

## 28. Same-Origin Policy

The **Same-Origin Policy** is a browser security mechanism that restricts how scripts from one origin interact with resources from another origin.

An origin is determined by:

    scheme + host + port

For example:

    https://example.com

and:

    http://example.com

are different origins because their schemes differ.

---

## 29. CORS

**CORS (Cross-Origin Resource Sharing)** is a browser mechanism that allows servers to specify which cross-origin requests are permitted.

The server can return headers such as:

    Access-Control-Allow-Origin

### Important Interview Point

CORS is:

    browser-enforced access control for cross-origin requests

CORS is **not**:

    an authentication mechanism

Authentication and authorization are separate concerns.

---

## 30. XSS

**XSS (Cross-Site Scripting)** occurs when attacker-controlled content is executed as JavaScript in a user's browser.

A common risk is inserting untrusted input into HTML.

For example, using `innerHTML` with unsanitized user input can create an XSS vulnerability.

### General Defenses

- Avoid inserting untrusted HTML.
- Prefer safer text-based APIs such as `textContent` when HTML is not required.
- Sanitize untrusted HTML when HTML rendering is actually necessary.
- Use appropriate browser security policies.

---

## 31. Browser Performance Concepts

For frontend performance interviews, connect these ideas:

    Frequent events
        ↓
    unnecessary function executions
        ↓
    unnecessary computation
        ↓
    slower / less responsive UI

Tools and concepts that help control this include:

- Debouncing
- Throttling
- `requestAnimationFrame`
- Avoiding unnecessary DOM work
- Avoiding layout thrashing
- Efficient event handling

---

## 32. Interview Recognition

### DOM question

Ask:

> How does JavaScript interact with an HTML page?

Think:

    DOM

### Event question

Ask:

> Why did a parent element receive a child's event?

Think:

    Event bubbling

### Delegation question

Ask:

> How can one listener handle many child elements?

Think:

    Event Delegation

### Performance question

Ask:

> Why can repeated DOM layout operations be expensive?

Think:

    Reflow / layout

### Storage question

Ask:

> Where should browser-side data be stored?

Compare:

    localStorage
    sessionStorage
    Cookies

### Cross-origin question

Ask:

> Why is a browser blocking a request to another origin?

Think:

    Same-Origin Policy
    CORS