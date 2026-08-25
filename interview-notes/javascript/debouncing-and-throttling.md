# Debouncing and Throttling

## Why They Are Needed

Some browser events can fire very frequently:

- Typing
- Scrolling
- Resizing
- Mouse movement
- Dragging

Running an expensive function on every event can cause unnecessary computation and hurt performance.

**Debouncing** and **throttling** control how frequently that function is allowed to execute.

---

## Debouncing

### Definition

**Debouncing** delays function execution until events stop occurring for a specified amount of time.

Every new event **resets the timer**.

The function executes only when no new event occurs during the delay period.

### Mental Model

    Event
      ↓
    Start timer
      ↓
    New event?
      ↓
    Cancel previous timer
      ↓
    Start new timer
      ↓
    No new event
      ↓
    Timer finishes
      ↓
    Execute function

### Example

For a search input with a `500ms` debounce:

    R        → timer starts
    Ru       → timer resets
    Rud      → timer resets
    Rudr     → timer resets
    Rudra    → timer resets
    Rudraksh → timer resets

    User stops typing
            ↓
          500ms passes
            ↓
        search() runs once

### Basic Implementation

    function debounce(fn, delay) {
        let timer;

        return function (...args) {
            clearTimeout(timer);

            timer = setTimeout(() => {
                fn(...args);
            }, delay);
        };
    }

### Using Debounce

    const debouncedSearch = debounce(searchUsers, 500);

    input.addEventListener("input", (e) => {
        debouncedSearch(e.target.value);
    });

### Key Mechanism

The important part is:

    clearTimeout(timer);
    timer = setTimeout(...);

Each new event cancels the previous scheduled execution and creates a new one.

---

## Why `timer` Must Be Outside the Returned Function

Correct:

    function debounce(fn, delay) {
        let timer;

        return function (...args) {
            clearTimeout(timer);

            timer = setTimeout(() => {
                fn(...args);
            }, delay);
        };
    }

`debounce()` is called once and creates the `timer` variable.

The returned function closes over `timer`, allowing multiple calls to share the same timer.

    debounce()
       ↓
    creates timer
       ↓
    returns function
       ↓
    closure preserves access to timer
       ↓
    call 1 → uses timer
    call 2 → uses same timer
    call 3 → uses same timer

### What Goes Wrong If `timer` Is Inside

    function debounce(fn, delay) {
        return function (...args) {
            let timer;

            clearTimeout(timer);

            timer = setTimeout(() => {
                fn(...args);
            }, delay);
        };
    }

A new `timer` variable is created on every call.

Therefore, each call cannot access the previous call's timer, so the previous timeout cannot be cancelled.

### Interview Connection

> The closure allows the returned function to preserve access to the outer `timer` variable across multiple calls.

---

## Throttling

### Definition

**Throttling** limits how frequently a function can execute while events continue occurring.

Unlike debouncing, throttling does **not** wait for the events to stop.

### Mental Model

    Event
      ↓
    Function allowed?
      ↓
    Execute
      ↓
    Start cooldown
      ↓
    More events?
      ↓
    Ignore while cooldown is active
      ↓
    Cooldown finishes
      ↓
    Allow next execution

### Example

With a `200ms` throttle:

    Scroll event → execute
    Scroll event → blocked
    Scroll event → blocked
    200ms passes
    Scroll event → execute
    Scroll event → blocked
    200ms passes
    Scroll event → execute

### Basic Implementation

    function throttle(fn, delay) {
        let waiting = false;

        return function (...args) {
            if (waiting) return;

            fn(...args);

            waiting = true;

            setTimeout(() => {
                waiting = false;
            }, delay);
        };
    }

### Key Mechanism

    if (waiting) return;

If the function is currently inside its cooldown period, the event is ignored.

Once the delay finishes:

    waiting = false;

the next event can execute the function.

---

## Debounce vs Throttle

| Debouncing | Throttling |
|---|---|
| Waits until events stop | Executes during continuous events |
| Resets the timer on every event | Limits execution frequency |
| Usually executes once after activity stops | Can execute repeatedly at a controlled interval |
| Good for search input | Good for scrolling |

---

## Common Use Cases

### Debouncing

- Search inputs
- Autocomplete
- Resize handling after resizing stops
- Waiting for the user to finish typing

### Throttling

- Scroll events
- Mouse movement
- Dragging
- Continuous position updates

---

## Interview Recognition

Ask:

> **Do I want the function to run after the user stops interacting, or do I want to limit execution while the interaction continues?**

If you want execution **after activity stops**:

**Debounce**

If you want execution **at controlled intervals during activity**:

**Throttle**

---

## Performance Insight

Debouncing and throttling do not primarily "save memory."

Their main purpose is to:

> **Reduce unnecessary function executions and computational work, improving application responsiveness and performance.**

---

## Interview Takeaways

- **Debounce** waits for a period of inactivity before executing.
- Every new debounced event resets the timer.
- `clearTimeout()` is essential for resetting the debounce timer.
- A closure allows the returned debounce function to preserve access to the timer.
- **Throttle** limits how frequently a function can execute.
- Throttle can execute immediately and then enforce a cooldown period.
- Debounce is commonly used for **search and autocomplete**.
- Throttle is commonly used for **scrolling, dragging, and mouse movement**.