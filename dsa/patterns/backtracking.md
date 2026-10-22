# Backtracking Pattern

## 1. Core Idea

**Backtracking explores possible choices recursively, undoing each choice to explore alternative paths.**

It is commonly used when a problem asks us to find:

- All possible combinations or subsets.
- All permutations or arrangements.
- Valid configurations that satisfy constraints.
- One or more paths satisfying a condition.
- Whether a valid solution exists.

The fundamental process is:

```text
Choose
  ↓
Explore recursively
  ↓
Undo the choice
  ↓
Explore another possibility
```

Backtracking systematically explores a decision tree rather than trying every solution in an unstructured way.

---

## 2. The Three Essential Steps

### Step 1: Choose

Make one possible decision.

Examples:
- Include or exclude an element.
- Select an unused number.
- Choose a character.
- Place an item in a valid position.
- Move to a neighboring cell in a grid.

### Step 2: Explore

Make a recursive call to explore what happens after the choice.

The recursive call continues making decisions until it reaches a base case or has no valid choices left.

### Step 3: Undo

Restore the state to what it was before the choice.

For example:

```js
path.push(choice);

backtrack(nextState);

path.pop();
```

The `pop()` restores the previous state so that the next branch can be explored independently.

Not every problem uses `push()` and `pop()`. Some modify a board, mark an element as used, or update another variable. The important principle is to restore any state that must not carry over between branches.

---

## 3. The Decision Tree

A decision tree represents the possible choices at each step.

For example, if every element can either be included or excluded:

```text
                  Start
                 /     \
          Include A   Exclude A
             /             \
       Include B         Include B
       /      \          /      \
      ...     ...       ...     ...
```

Each root-to-leaf path represents one sequence of decisions.

Backtracking explores these paths recursively.

The number of branches depends on the problem:

- Two choices per step: potentially `2^n` paths.
- Multiple choices per step: potentially `n!` paths or another combinatorial number.
- Constraints and pruning may eliminate many paths before they reach a complete solution.

A decision tree is a useful mental model, but it does not mean we must explicitly construct a tree in code.

---

## 4. State Variables

A backtracking function usually maintains information describing the current state.

| Variable | Purpose |
|---|---|
| `path` / `current` | The partial solution being constructed |
| `index` | The current position or decision, when applicable |
| `start` | The earliest candidate index allowed in the next selection, when applicable |
| `used` | Tracks which elements have already been selected |
| `result` | Stores completed solutions |
| Other state | Tracks constraints such as remaining capacity, visited cells, or a target sum |

These variables are problem-dependent. A solution does not need all of them.

For example:
- Subsets often use `index`, `path`, and `result`.
- Combinations often use `start`, `path`, and `result`.
- Permutations often use `path`, `used`, and `result`.
- Grid traversal may use coordinates and a visited-state marker.

The goal is to keep enough state to know which choices are currently valid.

---

## 5. Base Cases

A base case determines when a recursive call should stop.

There is no single base case that works for every backtracking problem.

Common examples include:

### A. A complete solution has been constructed

Record the solution and return.

```js
if (isComplete()) {
    result.push([...path]);
    return;
}
```

Examples:
- A complete subset.
- A permutation containing every element.
- A combination that reaches the required size.

### B. A valid answer has been found

If the problem requires only one answer, return it or signal success.

```js
if (isSolution()) {
    return true;
}
```

The caller may then propagate success back through the recursive calls.

### C. No valid choices remain

Stop exploring the current branch.

This can happen when:
- A constraint has been violated.
- The remaining sum cannot reach the target.
- A required position has no valid candidate.
- A path reaches a dead end.

A branch that cannot produce a valid answer does not always need to be recorded.

---

## 6. Two Common Backtracking Structures

### A. Include / Exclude

Use this structure when each element has two decisions: select it or skip it.

```js
function backtrack(index) {
    if (isComplete(index)) {
        result.push([...path]);
        return;
    }

    // Include the current element
    path.push(nums[index]);
    backtrack(index + 1);

    // Undo the choice
    path.pop();

    // Exclude the current element
    backtrack(index + 1);
}
```

Both branches move to the next index.

The difference is whether the current element is added to `path`.

Typical application: generating subsets.

### B. Iterate Over Available Choices

Use a loop when multiple candidates can be chosen at the current step.

```js
function backtrack(start) {
    if (isComplete()) {
        result.push([...path]);
        return;
    }

    for (let i = start; i < candidates.length; i++) {
        const choice = candidates[i];

        // Choose
        path.push(choice);

        // Explore
        backtrack(i + 1);

        // Undo
        path.pop();
    }
}
```

This is a common structure for combinations where each candidate can be selected at most once and selections must follow input order.

The recursive argument depends on the problem. Some problems use `i + 1`, some use `i`, and others use a different state entirely.

---

## 7. Common Backtracking Variations

### A. Subsets

Goal: Generate all subsets.

Typical decisions:
- Include the current element.
- Exclude the current element.

Another approach uses a loop to add each possible next element.

### B. Combinations

Goal: Select a group of elements satisfying a size or value constraint.

Common technique:
- Maintain a starting index.
- Iterate over eligible candidates.
- Choose a candidate and recurse from an appropriate next index.
- Remove the candidate when returning.

A starting index can prevent generating the same combination in different orders.

### C. Permutations

Goal: Generate arrangements where the order of elements matters.

Common technique:
- Choose an element not already used in the current path.
- Mark it as used.
- Recurse.
- Remove it from the path and unmark it.

Typical state:

```js
path
used
```

A `used` array or set helps prevent selecting the same input position more than once in a permutation.

### D. Constraint-Based Search

Goal: Find configurations satisfying rules.

Examples include:
- N-Queens.
- Sudoku.
- Word Search.
- Other board and placement problems.

Common technique:
1. Select the next position.
2. Try each valid candidate.
3. Check whether the choice violates a constraint.
4. Recurse only if the choice remains valid.
5. Undo the choice before trying another candidate.

### E. Path or Grid Search

Goal: Find a path satisfying a condition.

Common technique:
- Track the current position.
- Mark visited cells when required.
- Explore valid neighboring positions.
- Restore the visited state when returning.

Whether cells may be revisited depends on the problem's rules.

---

## 8. Pruning

**Pruning means stopping a branch as soon as we know it cannot produce a valid answer.**

Without pruning, the algorithm may explore many unnecessary possibilities.

Examples:

### Target sum

If all remaining candidates are non-negative and the current sum already exceeds the target, further additions cannot bring it back down.

The branch can be stopped.

This condition is not valid when remaining choices can reduce the sum.

### Combinations

If too few candidates remain to reach the required combination size, stop exploring that branch.

### Constraint-based problems

If a placement violates a constraint, there is no reason to explore further choices under that placement.

### General principle

```text
Make a choice
    ↓
Check constraints
    ↓
Invalid or impossible to complete?
    ├── Yes → Stop this branch
    └── No  → Recurse
```

Effective pruning can reduce the search space substantially, although worst-case complexity may remain exponential.

---

## 9. Handling Duplicate Values

Some problems contain duplicate input values and require unique combinations or permutations.

Simply using `path` and `pop()` may generate duplicate answers.

A common technique is:

1. Sort the input when sorting is appropriate.
2. Skip duplicate candidates at the same decision level.

For example:

```js
if (i > start && nums[i] === nums[i - 1]) {
    continue;
}
```

This particular condition is used in common sorted combination problems. The meaning of `start` and the duplicate rule depend on the traversal structure.

For permutations, duplicate handling usually requires a different condition involving which equal-valued elements have already been used.

Do not add duplicate-skipping logic automatically. First determine whether the problem requires unique answers and what makes two answers equivalent.

---

## 10. Returning an Answer vs Collecting Answers

Backtracking problems do not all return results in the same way.

### Collecting all solutions

Record every complete solution:

```js
if (isComplete()) {
    result.push([...path]);
    return;
}
```

Examples: Subsets, Permutations, Combination Sum.

### Finding one solution

Stop as soon as a valid solution is discovered.

```js
if (isSolution()) {
    return true;
}

if (exploreChoice()) {
    return true;
}

return false;
```

This is a conceptual structure. The exact implementation depends on how each choice is explored and undone.

### Counting solutions

Instead of storing every solution, accumulate counts or return a count from recursive calls.

The required output determines how the base case and recursive results should be handled.

---

## 11. General Backtracking Template

This is a conceptual template for collecting all solutions when choices can be iterated over:

```js
function backtrack(state) {
    if (isComplete(state)) {
        result.push([...path]);
        return;
    }

    for (const choice of getChoices(state)) {
        if (!isValid(choice, state)) {
            continue;
        }

        // Choose
        makeChoice(choice, state);
        path.push(choice);

        // Explore
        backtrack(nextState(state));

        // Undo
        path.pop();
        undoChoice(choice, state);
    }
}
```

Functions such as `getChoices`, `isValid`, `makeChoice`, `nextState`, and `undoChoice` are placeholders, not built-in JavaScript functions.

A real implementation may:
- Use include/exclude branches instead of a loop.
- Pass new state in function arguments.
- Mutate a shared path.
- Modify and restore a board.
- Return a boolean or count instead of collecting results.

The structure should fit the problem rather than forcing every problem into one template.

---

## 12. Complexity Analysis

Backtracking complexity depends on:

- The branching factor at each level.
- The maximum recursion depth.
- The number of valid solutions.
- The time required to validate each choice.
- How much pruning reduces the search space.
- Whether the output must store every solution.

Common possibilities include:

| Problem structure | Typical worst-case behavior |
|---|---|
| Two choices for each of `n` positions | `O(2^n)` search paths |
| Enumerating all permutations of `n` distinct elements | `O(n!)` solutions |
| Enumerating all subsets, including copying each subset | `O(n * 2^n)` time |
| Recursion depth of at most `n` | Usually `O(n)` stack space |

These are examples, not universal guarantees.

If all solutions must be returned, output space can be much larger than the auxiliary space. State that distinction when analyzing complexity.

Pruning improves practical performance but does not necessarily improve worst-case complexity.

---

## 13. Common Mistakes

- Forgetting to undo a choice after a recursive call.
- Using an incorrect base case.
- Continuing to explore after a branch is known to be invalid.
- Forgetting to advance the state or index, causing infinite recursion.
- Advancing the index incorrectly and skipping valid choices.
- Reusing mutable state without restoring it.
- Saving a mutable path directly instead of copying it.
- Generating duplicate answers when only unique results are allowed.
- Assuming include/exclude is appropriate for every problem.
- Assuming every backtracking solution needs a `used` array.
- Returning immediately when the problem requires collecting all valid solutions.
- Failing to account for output space in complexity analysis.

---

## 14. Recognition Clues

Consider backtracking when the problem asks you to:

- Generate all possible choices or arrangements.
- Return every valid combination.
- Explore multiple possible decisions.
- Search for a valid configuration.
- Construct paths subject to constraints.
- Find one valid solution among many candidates.

Common problem phrases:

```text
All possible
Every combination
All permutations
Generate subsets
Find all valid arrangements
Find a path satisfying constraints
Determine whether a valid configuration exists
```

These clues are not proof that backtracking is required. First identify the decisions, constraints, and output requirements.

---

## 15. Questions to Ask Before Coding

1. What exactly am I constructing or searching for?
2. What state must each recursive call know?
3. What choices are available at each step?
4. What makes a solution complete?
5. When should a branch stop early?
6. What state changes when I make a choice?
7. How do I restore that state when returning?
8. Do I need one answer, all answers, or a count?
9. Can duplicate answers occur?
10. What are the time, auxiliary-space, and output-space complexities?

---

## Key Takeaway

**Backtracking is a recursive search technique that explores choices, stops invalid or completed branches appropriately, and restores state to explore alternatives.**

The reusable mental model is:

```text
Define the state
      ↓
Check the base case
      ↓
Consider available choices
      ↓
Validate or prune each choice
      ↓
Choose
      ↓
Recurse
      ↓
Undo
      ↓
Try another choice
```

Subsets, combinations, permutations, and constraint-based search all use this same general idea, with different state definitions, choices, stopping conditions, and pruning rules.