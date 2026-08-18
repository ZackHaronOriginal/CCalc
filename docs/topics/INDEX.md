# Topic index

The canonical list. Every tracked item — research, feature, bug, question,
decision — is filed under exactly one of these.

Topics mirror the modules in `libs/`, plus three cross-cutting ones. This is
deliberate: the module structure is already the project's spine, so there is
never a debate about where something belongs, and the code and its history are
organised identically.

| Topic | Layer | Covers | Module |
|---|---|---|---|
| [`core`](core.md) | L0 | shared types, `Value`, `Diagnostic`, config, logging | `libs/core/` |
| [`expr`](expr.md) | L1 | lexing, parsing, the syntax tree, the grammar | `libs/expr/` |
| [`numeric`](numeric.md) | L1 | number types, matrices, precision, algorithms | `libs/numeric/` |
| [`units`](units.md) | L1 | dimensions, unit tables, conversion | `libs/units/` |
| [`eval`](eval.md) | L2 | evaluation, environment, variables, scope | `libs/eval/` |
| [`functions`](functions.md) | L2 | built-in functions and the registry | `libs/functions/` |
| [`plot`](plot.md) | L3 | sampling, scales, ticks, plot geometry | `libs/plot/` |
| [`session`](session.md) | L3 | history, workspaces, persistence | `libs/session/` |
| [`plugins`](plugins.md) | L0 + L3 | the ABI, the host, plugin authoring | `libs/plugin-abi/`, `libs/plugin-host/` |
| [`cli`](cli.md) | app | the terminal frontend | `apps/cli/` |
| [`gui`](gui.md) | app | the desktop frontend | `apps/gui/` |
| [`build`](build.md) | — | CMake, CI, packaging, tooling | `cmake/`, `.github/`, `packaging/` |
| [`project`](project.md) | — | structure, conventions, process | — |

## Adding a topic

1. Add a row here
2. Write `<topic>.md` — scope, exclusions, and how to tell it from its neighbours
3. Same commit

A topic is justified by a **new module** or a **genuinely new cross-cutting
concern**. It is not justified by one item that is awkward to classify — file
that under the closest fit and note the awkwardness.

## When topics get crowded: categories

**Not yet.** When an area accumulates so many topic folders that its index is
hard to scan, group related topics into a category folder:

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

Likely first categories: `engine` (core, expr, numeric, units, eval, functions),
`interface` (cli, gui, plot), `platform` (plugins, build, session).

**Do not create these in advance.** An empty hierarchy is harder to navigate than
a flat one, and the natural grouping will be obvious by the time it is needed.
