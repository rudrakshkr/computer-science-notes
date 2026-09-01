# React Hooks

## Core Idea

**Hooks** are functions that let functional React components use React features such as:

- State
- Effects
- Refs
- Context
- Reducers
- Memoization

Common built-in Hooks:

- `useState`
- `useEffect`
- `useRef`
- `useContext`
- `useReducer`
- `useMemo`
- `useCallback`

---

## Rules of Hooks

There are two fundamental rules:

1. **Only call Hooks at the top level.**
2. **Only call Hooks from React function components or custom Hooks.**

### Do Not Call Hooks Conditionally

Incorrect:

    if (isLoggedIn) {
        useEffect(() => {
            // ...
        }, []);
    }

Correct:

    useEffect(() => {
        if (isLoggedIn) {
            // ...
        }
    }, [isLoggedIn]);

### Why?

React relies on Hooks being called in a **consistent order on every render**.

For example:

    First render:
    useState()
    useEffect()

    Next render:
    useState()
    useEffect()

React can associate each Hook call with the correct internal state/effect.

If Hooks are called conditionally, the order can change between renders and React can no longer reliably associate Hook calls with their previous data.

### Interview Takeaway

> Hooks must be called in the same order on every render so React can correctly associate each Hook call with its corresponding state or effect.

---

# `useRef`

## Definition

`useRef` creates a mutable value that **persists across renders without causing a re-render when it changes**.

Example:

    const countRef = useRef(0);

The value is accessed through:

    countRef.current

---

## `useRef` vs `useState`

### `useState`

    const [count, setCount] = useState(0);

Updating state:

    setCount(1);

causes React to schedule a re-render.

### `useRef`

    const countRef = useRef(0);

Updating:

    countRef.current = 1;

does **not** cause a re-render.

### Key Difference

    useState
    → persists across renders
    → updating it triggers a render

    useRef
    → persists across renders
    → changing .current does not trigger a render

---

## Common Uses of `useRef`

`useRef` is commonly used for:

- DOM references
- Timer IDs
- Previous values
- Mutable values that do not need to appear immediately in the UI

---

## DOM References

Example:

    function SearchBox() {
        const inputRef = useRef(null);

        return (
            <>
                <input ref={inputRef} />

                <button onClick={() => inputRef.current.focus()}>
                    Focus
                </button>
            </>
        );
    }

After mounting:

    inputRef.current

points to the actual DOM `<input>` element.

Then:

    inputRef.current.focus();

can directly call the DOM element's `focus()` method.

### Why `useRef`?

The DOM reference needs to persist across renders, but changing the reference does not need to update the React UI.

---

## `useRef` and Rendering

Consider:

    function Counter() {
        const countRef = useRef(0);

        const handleClick = () => {
            countRef.current += 1;
            console.log(countRef.current);
        };

        return (
            <button onClick={handleClick}>
                {countRef.current}
            </button>
        );
    }

Clicking three times can produce:

    Console:
    1
    2
    3

But the button can continue displaying:

    0

because changing `countRef.current` does not trigger a re-render.

### Interview Takeaway

> A ref stores mutable data that survives renders without making that data part of React's rendered state.

---

## Timer IDs with `useRef`

A timer ID can be stored in a ref:

    const timerRef = useRef(null);

For example:

    timerRef.current = setTimeout(() => {
        // ...
    }, 500);

Later:

    clearTimeout(timerRef.current);

`useRef` is appropriate because the timer ID needs to persist across renders but does not need to trigger a UI update.

---

# `useContext`

## Definition

`useContext` lets a component consume a value from the nearest matching Context Provider without passing that value manually through every intermediate component.

It is commonly used to avoid **prop drilling**.

Example:

    const ThemeContext = createContext("light");

    function App() {
        return (
            <ThemeContext.Provider value="dark">
                <Navbar />
            </ThemeContext.Provider>
        );
    }

Inside `Navbar`:

    const theme = useContext(ThemeContext);

---

## Prop Drilling

Without Context:

    Provider
        ↓
      App
        ↓
     Layout
        ↓
     Navbar

The value may need to be passed through components that do not actually use it.

With Context:

    Provider
        ↓
      Navbar
        ↓
    useContext()

The intermediate components do not need to manually pass the value.

### Interview Takeaway

> `useContext` allows components to consume values from a Context Provider without explicitly passing those values through intermediate components.

---

## Context Updates

When the provided context value changes, components consuming that context can re-render with the new value.

For example:

    value = "dark"

changes to:

    value = "light"

Consumers using that context can update accordingly.

---

# `useReducer`

## Definition

`useReducer` is a Hook for managing **complex or related state transitions**.

Basic structure:

    const [state, dispatch] = useReducer(
        reducer,
        initialState
    );

A reducer receives:

    current state
    +
    action

and returns:

    new state

---

## Reducer

Example:

    function reducer(state, action) {
        switch (action.type) {
            case "increment":
                return {
                    ...state,
                    count: state.count + 1
                };

            default:
                return state;
        }
    }

---

## Dispatch

An action can be dispatched:

    dispatch({ type: "increment" });

The `dispatch` function sends the action to the reducer.

The reducer decides how the state should change.

### Flow

    dispatch(action)
          ↓
    reducer(state, action)
          ↓
    new state
          ↓
    React re-renders

### Interview Takeaway

> `dispatch` describes what happened; the reducer determines how the state should change.

---

## When `useReducer` Is Useful

It can be useful when state contains multiple related values and many possible transitions.

For example:

    loading
    data
    error

Possible actions:

    FETCH_START
    FETCH_SUCCESS
    FETCH_ERROR
    RESET

Instead of scattering state-transition logic across many setters, a reducer can centralize those transitions.

### `useState` vs `useReducer`

    useState
    → simpler state and straightforward updates

    useReducer
    → complex state logic or many related state transitions

`useReducer` is not automatically better than `useState`.

---

# Custom Hooks

## Definition

A **Custom Hook** is a reusable function that uses React Hooks to encapsulate reusable stateful logic.

Custom Hooks conventionally begin with:

    use

Example:

    useWindowWidth()

---

## Why Use Custom Hooks?

Suppose several components need the same:

- `useEffect`
- Event listener
- State management
- Cleanup logic

Instead of duplicating that code, extract it into a Custom Hook.

Example:

    function useWindowWidth() {
        const [width, setWidth] = useState(window.innerWidth);

        useEffect(() => {
            const handleResize = () => {
                setWidth(window.innerWidth);
            };

            window.addEventListener(
                "resize",
                handleResize
            );

            return () => {
                window.removeEventListener(
                    "resize",
                    handleResize
                );
            };
        }, []);

        return width;
    }

Then a component can use:

    const width = useWindowWidth();

### Important

A Custom Hook **reuses logic, not state between components**.

If two different components call:

    useWindowWidth()

each component gets its own Hook state.

Custom Hooks must also obey the Rules of Hooks.

---

# Focusing a DOM Element After Rendering

When a DOM action needs to happen after rendering, use a ref to obtain the DOM node and an effect to perform the action.

Example:

    function SearchBox() {
        const inputRef = useRef(null);

        useEffect(() => {
            inputRef.current?.focus();
        }, []);

        return <input ref={inputRef} />;
    }

The sequence is:

    Render component
        ↓
    React commits the DOM
        ↓
    inputRef.current points to input
        ↓
    useEffect runs
        ↓
    focus()

The key idea is:

> Use `useRef` for the DOM reference and `useEffect` for the post-render side effect.

---

# `useLayoutEffect` — Basic Awareness

`useLayoutEffect` is similar to `useEffect`, but it runs at a different point in the browser rendering lifecycle.

It is relevant when DOM measurements or mutations need to happen **before the browser paints**.

For ordinary side effects, prefer `useEffect`.

---

# Hook Selection Cheat Sheet

    Need component state?
    → useState

    Need a side effect / external synchronization?
    → useEffect

    Need a persistent mutable value without re-rendering?
    → useRef

    Need shared value without prop drilling?
    → useContext

    Need complex state transitions?
    → useReducer

    Need to memoize an expensive computed value?
    → useMemo

    Need a stable function reference?
    → useCallback

    Need to reuse stateful logic?
    → Custom Hook

---

# Interview Takeaways

- Hooks let functional components use React features such as state, effects, refs, context, reducers, and memoization.
- Hooks must be called at the top level and in a consistent order on every render.
- `useRef` persists a mutable value across renders without triggering re-renders when `.current` changes.
- `useRef` is commonly used for DOM references and timer IDs.
- `useContext` avoids prop drilling by allowing components to consume values from a Context Provider.
- `useReducer` centralizes complex or related state transitions.
- `dispatch` sends an action; the reducer determines the resulting state.
- Custom Hooks encapsulate and reuse stateful logic.
- Custom Hooks also obey the Rules of Hooks.
- `useMemo` memoizes a computed value.
- `useCallback` memoizes a function reference.
- Memoization should be used selectively rather than everywhere.
- `useRef` can provide DOM access without making the DOM node part of React state.
- `useEffect` can perform post-render DOM side effects when used together with a ref.