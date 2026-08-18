# Topics

Every tracked item — research, feature, bug, question, decision — is filed under
a **topic**. This file is the single source of truth for what the topics are.

Using one shared list across all areas is the point. It means a bug in the
parser, the research behind it, the feature that caused it and the decision that
settled it are all filed under `expr`, and you can find the whole story from any
one of them.

## The canonical list

Topics mirror the modules in `libs/`, plus three cross-cutting ones. This is
deliberate: the module structure is already the project's spine, so there is
never a debate about which topic something belongs to.

| Topic | Covers |
|---|---|
| `core` | shared types, `Value`, `Diagnostic`, config, logging |
| `expr` | lexing, parsing, the syntax tree, the grammar |
| `numeric` | number types, matrices, precision, numerical algorithms |
| `units` | dimensions, unit tables, conversion |
| `eval` | evaluation, environment, variables, scope |
| `functions` | built-in functions and the registry |
| `plot` | sampling, scales, ticks, plot geometry |
| `session` | history, workspaces, persistence |
| `plugins` | the ABI, the host, plugin authoring |
| `cli` | the terminal frontend |
| `gui` | the desktop frontend |
| `build` | CMake, CI, packaging, tooling |
| `project` | structure, conventions, process — anything spanning modules |

## Rules

- **Use an existing topic if one fits.** Inventing a near-duplicate (`parser`
  next to `expr`) destroys the value of a shared list.
- **A new topic needs a new module, or a genuinely new cross-cutting concern.**
  Add it here first, in the same commit.
- **One topic per item.** If something truly spans two, file it under the one
  that would have to change, and mention the other in the item's `Related` field.
  Cross-references are cheap; duplicated items are not.
- `project` is for things that span modules — not a dumping ground for anything
  hard to classify. If you reach for it twice in a row, the topic list probably
  needs a real addition.

## When topics get crowded: categories

Not yet. When a single area accumulates so many topics that its index is hard to
scan, group related topics into a category folder:

```
research/
├── engine/          ← category
│   ├── expr/
│   ├── eval/
│   └── numeric/
└── interface/
    ├── cli/
    └── gui/
```

Likely first categories, when the time comes: `engine` (core, expr, numeric,
units, eval, functions), `interface` (cli, gui, plot), `platform` (plugins,
build, session).

**Do not create these in advance.** An empty hierarchy is harder to navigate
than a flat one, and the natural grouping will be obvious by the time it is
actually needed.
