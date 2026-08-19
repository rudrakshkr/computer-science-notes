# JavaScript Modules

## What Are Modules?

A module is a JavaScript file whose code can be explicitly shared with other files using `export` and `import`.

```text
module A
   ↓ export
module B
   ↓ import
```

Benefits:

- Encapsulation
- Avoiding global namespace pollution
- Separating responsibilities
- Managing dependencies

---

## Named Exports

A module can have multiple named exports.

```js
export function add(a, b) {
    return a + b
}

export function subtract(a, b) {
    return a - b
}
```

Import:

```js
import { add, subtract } from "./math.js"
```

Can also rename:

```js
import { add as sum } from "./math.js"
```

---

## Default Export

A module can have **one default export**.

```js
export default function add(a, b) {
    return a + b
}
```

Import:

```js
import add from "./math.js"
```

The importer can choose the local name:

```js
import calculate from "./math.js"
```

### Remember

```text
Named export
→ import { add }

Default export
→ import add
```

---

## Module Scope

Each module has its own scope.

```js
// math.js

const secret = 42

export const add = (a, b) => a + b
```

`secret` cannot be accessed from another module unless it is exported.

```text
Top-level variable in module
→ module-scoped
→ not automatically global
```

---

## Namespace Import

Import all named exports under one namespace:

```js
import * as math from "./math.js"

math.add(2, 3)
math.subtract(5, 2)
```

---

# ES Modules vs CommonJS

## ES Modules (ESM)

Uses:

```js
import
export
```

Example:

```js
// math.js

export function add(a, b) {
    return a + b
}
```

```js
// app.js

import { add } from "./math.js"
```

---

## CommonJS (CJS)

Uses:

```js
require()
module.exports
```

Example:

```js
// math.js

function add(a, b) {
    return a + b
}

module.exports = { add }
```

```js
// app.js

const { add } = require("./math.js")
```

---

## ESM vs CommonJS

| ES Modules | CommonJS |
|---|---|
| `import` / `export` | `require()` / `module.exports` |
| Modern JavaScript module system | Older Node.js module system |
| Statically structured | More dynamic |
| Common in modern frontend and Node.js | Historically common in Node.js |

### Important

Don't say:

```text
CommonJS = backend only
```

Modern Node.js supports ES Modules as well.

---

## Static vs Dynamic Structure

ES Modules have a more static dependency structure:

```js
import { add } from "./math.js"
```

This allows tooling to analyze dependencies and enables optimizations such as **tree-shaking**.

-> **Tree shaking** is a form of dead code elimination used by modern JavaScript bundlers to remove unused code from the final production bundle

CommonJS is more dynamic:

```js
const math = require("./math.js")
```

and can be used conditionally:

```js
if (useMath) {
    const math = require("./math.js")
}
```

---

# Key Interview Takeaways

1. Modules let files share code explicitly.
2. Named exports use the exported name.
3. Default exports can be imported using any local name.
4. A module has its own scope.
5. `import * as name` creates a namespace containing named exports.
6. ES Modules use `import` / `export`.
7. CommonJS uses `require()` / `module.exports`.
8. ESM has a more static dependency structure, which enables tooling optimizations such as tree-shaking.
9. A module can have multiple named exports but only one default export.