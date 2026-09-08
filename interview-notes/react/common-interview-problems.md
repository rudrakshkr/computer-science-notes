# React — Common Interview Problems

## 1. Parent & Child Re-rendering

When a parent component re-renders, its child components **re-render by default**, even if their props have not changed.

`React.memo` can prevent this when the child's props are unchanged.

---

## 2. `React.memo`

`React.memo` memoizes a component and allows React to skip re-rendering it when its **props have not changed**.

It does not prevent all re-renders.

A memoized component can still re-render because of:

- its own state changes
- context changes
- changed props

---

## 3. Object Props + `React.memo`

Objects are compared by **reference**, not by their contents.

If an object is created during every parent render:

```text
Parent re-renders
→ new object created
→ new object reference
→ React.memo sees changed prop
→ Child re-renders
```

Use `useMemo` when the object/value should keep the same reference until its dependencies change.

**Key distinction:**

`useMemo` → memoizes a **value/reference**

---

## 4. Function Props + `React.memo`

A function created inside the parent is also a new reference on every parent render.

Therefore:

```text
Parent re-renders
→ new function reference
→ Child receives changed prop
→ React.memo cannot skip the render
```

`useCallback` can preserve the function reference until its dependencies change.

**Key distinction:**

`useCallback` → memoizes a **function reference**

---

## 5. State Update Batching

React can batch multiple state updates.

Consider:

```text
setCount(count + 1)
setCount(count + 1)
setCount(count + 1)
```

If `count` is `0`, all three updates use the same value from that render:

```text
setCount(1)
setCount(1)
setCount(1)
```

Final value → `1`

When multiple updates depend on the previous state, use the **functional updater**:

```text
setCount(prev => prev + 1)
```

Each update receives the latest pending state.

---

## 6. `useEffect` Dependencies

The dependency array determines when an effect should re-run based on dependency changes.

With:

```text
[]
```

the effect runs on the initial mount and does not re-run because of dependency changes.

If an effect uses a reactive value such as `userId`, that value should generally be included in the dependency array:

```text
[userId]
```

Then the effect re-runs when `userId` changes.

---

## 7. `useEffect` Cleanup

The function returned from `useEffect` is the **cleanup function**.

It is used to clean up resources such as:

- timers
- subscriptions
- event listeners
- connections

For an effect with `[]`, cleanup normally runs when the component **unmounts**.

With dependencies, React runs the previous cleanup **before running the effect again** after a dependency change.

Example flow:

```text
Effect for room 1
→ roomId changes
→ cleanup: disconnect room 1
→ new effect: connect room 2
```

Important:

> The cleanup function does not run immediately after the effect finishes. It runs later when the previous effect needs to be cleaned up.

---

## 8. Stale Closures

A **stale closure** occurs when a callback created during an earlier render keeps using an outdated state or prop value.

Example:

```text
count = 0
↓
effect runs once
↓
setInterval captures count = 0
↓
count changes to 1, 2, 3
↓
setInterval still sees 0
```

With `setInterval`, the callback can therefore keep logging the old value until the interval is cleaned up.

### Key Insight

The problem is the callback's **captured value**, not whether the state update uses a functional updater.

---

## 9. Controlled vs Uncontrolled Inputs

### Controlled

React state is the **source of truth**:

```text
React state
↓
input value
↓
onChange
↓
state update
```

Example:

```text
<input value={name} onChange={...} />
```

### Uncontrolled

The DOM maintains the input's value, usually accessed through a ref.

**Key takeaway:**

> Controlled input → React controls the value.  
> Uncontrolled input → DOM controls the value.

---

## 10. Derived State

Do not store a value in state when it can be calculated directly from existing props or state.

For example:

```text
firstName
lastName
↓
fullName
```

`fullName` is **derived data**, so storing it separately creates unnecessary state and can cause an extra render.

Prefer calculating it directly during rendering.

**Key takeaway:**

> Avoid redundant state when the value can be derived from existing data.

---

## 11. State vs `useRef`

### State

Changing state:

- schedules a re-render
- allows the updated value to affect the UI

### Ref

Changing `ref.current`:

- does not trigger a re-render
- persists across renders

### Rule of Thumb

> Use **state** for values that affect the UI.  
> Use **refs** for values that need to persist across renders without triggering a render.

---

## Key Interview Takeaways

### `React.memo`

Prevents unnecessary child re-renders when props are unchanged.

### `useMemo`

Memoizes a value/reference.

### `useCallback`

Memoizes a function reference.

### Functional State Update

Use when the new state depends on the previous state.

### `useEffect`

Synchronizes a component with external systems and uses dependencies to determine when it should re-run.

### Cleanup

Tears down resources from the previous effect.

### Stale Closure

A callback keeps using an outdated value captured from an earlier render.

### Derived State

Don't store data that can be calculated from existing state/props.

### State vs Ref

State changes → re-render.  
Ref changes → no re-render.