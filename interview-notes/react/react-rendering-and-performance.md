# React Rendering & Performance

## Rendering

A React component **renders** when React calls the component function to determine what the UI should look like.

A render does not automatically mean the browser's DOM is changed.

Conceptually:

    State / Props update
          ↓
    Component renders
          ↓
    New element tree
          ↓
    Reconciliation
          ↓
    Commit necessary changes
          ↓
    Actual DOM

---

## What Causes a Re-render?

Common causes include:

- A component's state changes.
- A parent component renders again.
- A component receives changed props.
- A consumed context value changes.

A component can therefore re-render even when its own state has not changed.

---

## Parent → Child Rendering

Normally, when a parent component renders, its child components are also rendered.

Example:

    Parent
      ↓
    Child

When the parent's state changes:

    Parent state changes
          ↓
    Parent renders
          ↓
    Child renders

`React.memo` can prevent some unnecessary child renders when the child's props have not changed.

---

## Render Phase

The **render phase** is when React calculates what the UI should look like.

React:

- Calls components.
- Evaluates JSX.
- Produces a new element tree.
- Determines what changed compared with the previous render.

The render phase should be treated as **calculation**, not as the place to directly mutate the DOM.

---

## Commit Phase

The **commit phase** is when React applies the necessary changes to the actual DOM.

Conceptually:

    Render
      ↓
    Reconciliation
      ↓
    Commit
      ↓
    DOM updated

### Interview Distinction

> **Render phase = calculate the new UI.**

> **Commit phase = apply the required DOM changes.**

A component rendering does not necessarily mean that the actual DOM changes.

---

## Reconciliation

**Reconciliation** is the process React uses to compare the newly rendered element tree with the previous one and determine what needs to change.

Example:

Previous:

    <div>
        <h1>Hello</h1>
        <p>World</p>
    </div>

New:

    <div>
        <h1>Hello</h1>
        <p>React</p>
    </div>

React can determine:

    <div>      → same
    <h1>       → same
    "Hello"    → same
    <p>        → same element
    "World"    → changed

The necessary DOM update is therefore only the changed text.

---

## Virtual DOM / Element Tree

The term **Virtual DOM** is commonly used to describe React's lightweight representation of the UI.

A useful interview mental model is:

    Previous element tree
            ↓
    New element tree
            ↓
    Reconciliation
            ↓
    Required DOM updates

Avoid saying:

> React compares the actual DOM directly with the Virtual DOM.

A more precise explanation is:

> React creates a new element tree during rendering, reconciles it with the previous tree, and commits the necessary changes to the actual DOM.

---

## Keys

Keys provide **stable identity for elements in a list**.

Example:

    {users.map((user) => (
        <li key={user.id}>{user.name}</li>
    ))}

React uses keys to match list items between renders.

### Why Keys Matter

Suppose:

    Alice   → key 101
    Bob     → key 205
    Charlie → key 309

After removing Bob:

    Alice   → key 101
    Charlie → key 309

The identities remain stable.

---

## Index as a Key

Using the array index as a key can cause problems when a list is:

- Reordered
- Inserted into
- Deleted from

Example:

    Before:
    Alice   → key 0
    Bob     → key 1
    Charlie → key 2

After removing Bob:

    Alice   → key 0
    Charlie → key 1

Key `1` previously represented Bob but now represents Charlie.

This can cause React to associate the wrong component instance/state with a list item.

### Important Nuance

Index keys are not inherently forbidden.

They can be acceptable when the list is:

- Static
- Never reordered
- Never inserted into
- Never deleted from

For dynamic lists, prefer a stable identifier belonging to the item.

---

## State Preservation

React preserves state based on a component's **identity and position in the rendered tree**.

If the same component remains at the same position, React can preserve its state.

Example:

    <Counter />

If the same `Counter` remains in the same position across renders:

    count = 3
        ↓
    re-render
        ↓
    count = 3

---

## State Reset When Removed

Consider:

    {show && <Counter />}

If `show` becomes `false`:

    Counter
       ↓
    removed from tree

When it is later rendered again:

    new Counter instance
       ↓
    state starts from initial value

Removing a component from the rendered tree can therefore reset its state.

---

## Same Component at Same Position

Consider:

    {show ? <Counter /> : <Counter />}

Both branches produce the same component type at the same position.

Therefore, changing `show` does not necessarily reset the `Counter` state.

The state remains associated with that component's identity and position.

---

## Keys Can Reset State

Changing a component's key can give it a new identity.

Example:

    <Counter key="A" />

then:

    <Counter key="B" />

React treats these as different component identities.

Conceptually:

    key A
      ↓
    old component
      ↓
    key changes
      ↓
    old component unmounted
      ↓
    new component mounted
      ↓
    state resets

### Interview Takeaway

> Changing a component's key can intentionally reset its state.

---

# React Performance

## `React.memo`

`React.memo` memoizes a component and allows React to skip rendering it when its parent renders **but its props have not changed**.

Example:

    const Child = React.memo(function Child({ name }) {
        return <p>{name}</p>;
    });

If the parent renders again with the same `name` prop, React can skip the child render.

---

## What `React.memo` Does Not Prevent

`React.memo` does not mean:

> The component will never render again.

A memoized component can still render when:

- Its own state changes.
- Its props change.
- A context value it consumes changes.

For example:

    Parent re-renders
          ↓
    Props unchanged
          ↓
    React.memo
          ↓
    Child render can be skipped

But:

    Child state changes
          ↓
    Child re-renders

---

## Shallow Prop Comparison

`React.memo` uses shallow comparison of props.

Primitive values are compared by value.

Objects, arrays, and functions are compared by reference.

Example:

    Previous:
    [1, 2, 3] → object A

    New render:
    [1, 2, 3] → object B

Even though the contents are identical:

    objectA !== objectB

Therefore, React considers the prop changed.

---

## Arrays / Objects and `React.memo`

Consider:

    function Parent() {
        const [count, setCount] = useState(0);

        const numbers = [1, 2, 3];

        return <Child numbers={numbers} />;
    }

Every render creates a new array.

Therefore:

    Parent re-renders
          ↓
    new array created
          ↓
    new reference
          ↓
    Child receives changed prop
          ↓
    React.memo cannot skip Child

The values can be identical while the reference is different.

---

## Functions and `React.memo`

The same issue occurs with functions.

Example:

    const handleClick = () => {
        console.log("clicked");
    };

Every render creates a new function reference.

Therefore:

    previous function !== new function

A memoized child receiving that function as a prop can re-render because its prop reference changed.

---

## `useMemo`

`useMemo` memoizes a **computed value**.

Example:

    const expensiveValue = useMemo(() => {
        return calculateSomething(a, b);
    }, [a, b]);

React can reuse the previously calculated value until a dependency changes.

### Important

Do not use `useMemo` for every calculation.

Memoization has its own overhead and increases code complexity.

Use it when:

- A computation is sufficiently expensive.
- Reusing the computed value is beneficial.
- Stable referential identity of the value is important.

---

## `useCallback`

`useCallback` memoizes a **function reference**.

Example:

    const handleClick = useCallback(() => {
        console.log("clicked");
    }, []);

As long as its dependencies do not change, React can reuse the same function reference across renders.

---

## `useMemo` vs `useCallback`

    useMemo
    → memoizes a returned value

    useCallback
    → memoizes a function reference

### Example

Value:

    const result = useMemo(() => calculate(), [dependency]);

Function:

    const handleClick = useCallback(() => {
        doSomething();
    }, [dependency]);

---

## `useCallback` + `React.memo`

These concepts are often used together.

Without `useCallback`:

    Parent renders
        ↓
    new function created
        ↓
    Child receives new function reference
        ↓
    React.memo cannot skip Child

With `useCallback`:

    Parent renders
        ↓
    same function reference
        ↓
    Child props remain unchanged
        ↓
    React.memo can skip Child

---

## `useMemo` + `React.memo`

Similarly, `useMemo` can preserve an object or array reference.

Example:

    const numbers = useMemo(() => [1, 2, 3], []);

    <Child numbers={numbers} />

The same array reference can then be passed to a memoized child across renders.

---

## Don't Overuse Memoization

Memoization is not automatically a performance improvement.

Using:

- `React.memo`
- `useMemo`
- `useCallback`

everywhere can:

- Add complexity.
- Make code harder to understand.
- Add memoization overhead.
- Provide little or no benefit for cheap calculations.

First identify an actual performance problem or a meaningful referential-stability requirement.

---

## Rendering vs DOM Update

An important React distinction:

    Component re-renders
    ≠
    Entire DOM is recreated

React can render a component again while making only a small DOM update.

Example:

    <h1>{count}</h1>
    <p>Hello</p>

If only `count` changes:

    <h1>0</h1>
    <p>Hello</p>

becomes:

    <h1>1</h1>
    <p>Hello</p>

The `<p>` does not need to be recreated.

---

## Interview Recognition

### Rendering question

Ask:

> What causes this component to render again?

Think about:

- State
- Props
- Parent render
- Context

### Reconciliation question

Ask:

> Does a component render mean that the DOM is rebuilt?

No.

React reconciles the new element tree with the previous one and commits only the necessary changes.

### Keys question

Ask:

> Why does this list item need a key?

Think:

    Stable identity across renders

### Performance question

Ask:

> Why did my memoized child re-render?

Check whether:

- A prop changed.
- An object/array reference changed.
- A function reference changed.
- The child's own state changed.
- Consumed context changed.

---

## Interview Takeaways

- Rendering calculates what the UI should look like.
- The commit phase applies necessary changes to the actual DOM.
- A component render does not necessarily mean a DOM update.
- Reconciliation compares the new element tree with the previous tree.
- Keys provide stable identity for list items.
- Index keys can cause problems in dynamic lists.
- React preserves state based on component identity and position in the tree.
- Removing a component can reset its state.
- Changing a key can intentionally reset state.
- `React.memo` can skip renders when props are unchanged.
- `React.memo` uses shallow prop comparison.
- Arrays, objects, and functions are compared by reference.
- `useMemo` memoizes a value.
- `useCallback` memoizes a function reference.
- `React.memo` + stable references can prevent unnecessary child renders.
- Memoization should be used selectively rather than everywhere.