# React Fundamentals

## Props

**Props** are data passed from a parent component to a child component.

Props can contain:

- Values
- Objects
- Arrays
- Functions
- Other data

The parent controls the props, while the child should treat them as **read-only**.

Conceptually:

    Parent
      ↓ props
    Child

---

## State

**State** is data managed by a React component that can change over time.

Example:

    const [count, setCount] = useState(0);

Here:

- `count` → current state value
- `setCount` → function used to request a state update

When state changes, React schedules a re-render so the UI can reflect the new state.

---

## Props vs State

| Props | State |
|---|---|
| Passed from parent to child | Managed by the component |
| Read-only from child's perspective | Updated using a state setter |
| Parent controls the value | Component controls the value |
| Used to pass data/behavior | Used for changing component data |

### Interview Takeaway

> **Props are passed into a component by its parent; state is managed by the component itself.**

---

## State Ownership

State should generally live in the component that needs to own and control it.

Example:

    function Parent() {
        const [count, setCount] = useState(0);

        return <Child count={count} />;
    }

Here:

    Parent → owns count
    Child  → receives count as a prop

The child cannot directly modify the parent's state.

If the child needs to update it, the parent can pass the setter:

    function Parent() {
        const [count, setCount] = useState(0);

        return (
            <Child
                count={count}
                setCount={setCount}
            />
        );
    }

---

## Lifting State Up

**Lifting state up** means moving state into the nearest common parent when multiple components need to share or control that state.

The parent then passes:

- The current value
- An updater function when necessary

This keeps the state in one place while allowing children to interact with it.

---

## State Updates

A state setter can receive either:

### A Direct Value

    setCount(10);

### A Functional Updater

    setCount((prev) => prev + 1);

Use the functional updater when the new state depends on the previous state.

---

## Functional State Updates

Consider:

    setCount(count + 1);
    setCount(count + 1);

If `count` is `0`, both updates can be based on the same current render value.

They can effectively request:

    setCount(1);
    setCount(1);

Instead, use:

    setCount((prev) => prev + 1);
    setCount((prev) => prev + 1);

Now React can apply them sequentially:

    0 → 1 → 2

### Interview Takeaway

> When the next state depends on the previous state, prefer the **functional updater form**.

---

## Local Variables vs State

A normal local variable inside a component is recreated whenever the component function executes.

Example:

    function Counter() {
        let count = 0;

        count++;
    }

A local variable:

- Is not tracked by React.
- Does not trigger a re-render when changed.
- Does not persist between renders.

React state behaves differently:

    const [count, setCount] = useState(0);

State:

- Persists across renders.
- Can trigger a re-render when updated.
- Is managed by React.

### Important

A local variable does not reset because of its scope. It resets because the **component function executes again and creates a new variable**.

---

## Re-rendering

A state update schedules the component for another render.

Example:

    setCount(count + 1);

Conceptually:

    State update
        ↓
    React schedules update
        ↓
    Component renders again
        ↓
    UI reflects new state

A state update does not directly modify the currently executing `count` variable.

---

## State Update Timing

Consider:

    console.log(count);

    setCount(count + 1);

    console.log(count);

If `count` starts at `0`, both logs can print:

    0
    0

The state setter requests an update, but the current render still sees its existing state value.

The new state becomes available during the subsequent render.

### Interview Takeaway

> State updates do not immediately change the state value captured by the currently executing render.

---

## Batching

**Batching** means React can group multiple state updates together and process them as a single render/commit cycle.

Example:

    setCount((prev) => prev + 1);
    setCount((prev) => prev + 1);
    setCount((prev) => prev + 1);

The functional updaters can be applied sequentially:

    0 → 1 → 2 → 3

React can then commit the resulting UI together instead of unnecessarily rendering after every individual update.

### Interview Takeaway

> Batching groups multiple state updates together to reduce unnecessary rendering work and improve performance.

---

## Parent and Child Re-rendering

When a parent component re-renders, its children are normally rendered again as part of that render.

Example:

    Parent
      ↓
    Child

If the parent's state changes:

    Parent state changes
          ↓
    Parent re-renders
          ↓
    Child renders again

This does not require the child to have its own state change.

Later, `React.memo` can allow certain child renders to be skipped.

---

## `useEffect`

`useEffect` is used for **side effects and synchronization with external systems**.

Common examples:

- API requests
- Event listeners
- Timers
- WebSocket connections
- Subscriptions
- Browser APIs

Example:

    useEffect(() => {
        // side effect
    }, []);

### Core Idea

React's render should primarily determine what the UI should look like.

Effects are used for work that needs to synchronize the component with something outside the render calculation.

---

## Dependency Array

The dependency array tells React which reactive values the effect depends on and when it should re-run.

### Empty Dependency Array

    useEffect(() => {
        // effect
    }, []);

The effect runs after the initial render and does not re-run because of later dependency changes.

### Dependency

    useEffect(() => {
        // effect
    }, [count]);

The effect runs after the initial render and again when `count` changes.

### No Dependency Array

    useEffect(() => {
        // effect
    });

The effect runs after every render.

### Important

An effect without a dependency array does **not automatically create an infinite loop**.

It becomes an infinite loop when the effect itself continuously causes another render.

Example:

    useEffect(() => {
        setCount(count + 1);
    });

This can create:

    render
      ↓
    effect
      ↓
    setCount()
      ↓
    render
      ↓
    effect
      ↓
    ...

---

## Effect Cleanup

An effect can return a cleanup function.

Example:

    useEffect(() => {
        const id = setInterval(() => {
            console.log("tick");
        }, 1000);

        return () => {
            clearInterval(id);
        };
    }, []);

The cleanup function reverses or removes the external work created by the effect.

Common cleanup tasks:

- Remove event listeners
- Clear timers
- Close connections
- Unsubscribe from subscriptions

---

## When Cleanup Runs

Cleanup runs:

1. When the component unmounts.
2. Before the effect runs again because its dependencies changed.

Example:

    useEffect(() => {
        console.log("Effect");

        return () => {
            console.log("Cleanup");
        };
    }, [count]);

When `count` changes:

    Cleanup
      ↓
    New Effect

The cleanup belongs to the **previous effect instance**.

---

## Effect Dependencies

Suppose:

    const [userId, setUserId] = useState(1);

    useEffect(() => {
        fetchUser(userId);
    }, [userId]);

When `userId` changes:

    userId = 1
        ↓
    userId = 2
        ↓
    effect runs again
        ↓
    fetchUser(2)

If `[]` were used instead:

    useEffect(() => {
        fetchUser(userId);
    }, []);

the effect would only run for the initial render and would not re-run when `userId` changes.

---

## Effects vs Event Handlers

An important React distinction:

**Event handler**

Used for responding directly to a user action.

    onClick={() => {
        // user action
    }}

**Effect**

Used for synchronizing with an external system after rendering.

    useEffect(() => {
        // synchronization
    }, [dependency])

Do not automatically put ordinary user-event logic inside an Effect.

---

## Interview Takeaways

- **Props** are data passed from parent to child.
- **State** is data managed by a component.
- State changes can schedule a re-render.
- Local variables are recreated on each render and are not React state.
- Use functional state updates when the next state depends on the previous state.
- React can batch multiple state updates.
- A parent re-render normally causes its children to render again.
- `useEffect` handles side effects and synchronization with external systems.
- `[]` means no dependency-driven re-runs after the initial effect execution.
- `[dependency]` causes re-running when that dependency changes.
- No dependency array means the effect runs after every render.
- Cleanup runs on unmount and before an effect re-runs due to dependency changes.