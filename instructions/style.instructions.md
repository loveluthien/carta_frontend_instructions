# CARTA frontend code style instructions

## Naming Conventions

- These rules are enforced by `@typescript-eslint/naming-convention` in `eslint.config.mjs` in the root directory. Violations cause ESLint errors. 
- Better not use abbreviations as identifiers, unless the abbreviation is widely known and used in the codebase (e.g. `URL`, `HTML`, `API`). 
- Don't use single letters as identifiers, except for loop indices (e.g. `i`, `j`, `k`).

### General Default

- All identifiers must use `camelCase`, `PascalCase`, or `UPPER_CASE`.
- Leading underscores (`_name`) are **allowed**.
- Trailing underscores are **forbidden**.

### Types, Classes, Interfaces, Enums, Type Aliases, Type Parameters

Use **`PascalCase`**.

```ts
class ImageStore { ... }
interface RegionConfig { ... }
enum CoordinateMode { ... }
type FrameId = number;
```

### Variables, Parameters, and Functions

- Local variables and function names: **`camelCase`** (leading `_` allowed).
- Parameters: **`camelCase`** or **`PascalCase`** (leading `_` allowed).
- Global module-level `const` variables: **`UPPER_CASE`**.
- Global module-level `const` variables of type `function` (i.e. a function stored in a variable at module scope): **`PascalCase`**.

```ts
const frameCount = 5;                  // local, camelCase
const MAX_CHANNELS = 256;              // global primitive, UPPER_CASE
export const DEFAULT_ZOOM = 1.0;       // global exported, UPPER_CASE
const FormatLabel = (s: string) => s;  // global function variable, PascalCase
```

### Boolean Identifiers

Variables, parameters, properties, and accessors of **boolean type** must use **`PascalCase`** and one of these prefixes: `is`, `are`, `should`, `has`, `have`, `can`, `did`, `will` (leading `_` allowed).

```ts
isLoading: boolean;
shouldRender: boolean;
hasError: boolean;
canZoom: boolean;
didUpdate: boolean;
willUnmount: boolean;
```

### Class Members

| Member | Modifiers | Format |
|--------|-----------|--------|
| Instance method / property / accessor | — | `camelCase` |
| Protected property | `protected` | `camelCase` |
| Static property (not readonly) | `static` | `camelCase` |
| Static readonly property | `public static readonly` | `UPPER_CASE` |
| Static readonly property | `private static readonly` | `PascalCase` |
| Static accessor (`get`/`set`) | `public static` | `PascalCase` |
| Static method | `static` | `camelCase` |

```ts
class WidgetStore {
    // instance members — camelCase
    frameIndex = 0;
    get currentFrame() { ... }
    updateFrame() { ... }

    // public static readonly — UPPER_CASE
    public static readonly MAX_WIDGETS = 32;

    // private static readonly — PascalCase
    private static readonly DefaultConfig = { ... };

    // static property — camelCase
    public static instance: WidgetStore;

    // public static accessor — PascalCase
    public static get ActiveId() { ... }

    // static method — camelCase
    public static createStore() { ... }
}
```

**Always add `public` to `static` properties and methods if they are not private.**

### Exceptions

The following legacy identifiers and specific prefixes are exempted from the naming conventions and can be used with any casing:
- Legacy identifiers: `N`, `M`, `p`, `UIn8`, `Iz`, `Jys`, `SN`
- Specific prefixes: `CARTA`, `HDU`, `WCS`

**Don't add another exception.**

### Enum Members, Object Literal Properties/Methods, Type Properties/Methods

**No format restriction** — any casing is accepted.

```ts
enum Action { zoomIn, ZoomOut, RESET }   // all valid
const config = { "content-type": "json", maxItems: 5 };
```

### Import Sorting

Imports must be grouped and sorted by `eslint-plugin-simple-import-sort` in this order:

1. `react` and external packages (`@?\\w`)
2. Internal aliases: `components`, `enums`, `icons`, `models`, `scripting`, `services`, `stores`, `utilities`
3. Side-effect imports
4. Parent-directory relative imports (`../`)
5. Same-directory relative imports (`./`)
6. CSS / style imports

```ts
import React from "react";
import {observer} from "mobx-react";

import {AppStore} from "stores/AppStore";

import {MyHelper} from "./MyHelper";
import "./styles.scss";
```

### Type Imports

Always use **inline type imports** (`import {type Foo}` or `import type {Foo}`), enforced by `@typescript-eslint/consistent-type-imports`.

```ts
import {type RegionConfig} from "models";
```
