# Feature backlog

Every feature, one line each. **If it is not listed here, it is not planned.**

Next free ID: **F-021**. IDs are never reused, and do not change when a file
moves between topic folders.

## Planned

| ID | Topic path | Feature | Status | Milestone | Detail |
|---|---|---|---|---|---|
| F-001 | expression-engine / lexical-handling | Tokenise input into a position-carrying token stream | planned | M1 | — |
| F-002 | expression-engine / notation-handling | Parse infix notation into a syntax tree | planned | M1 | — |
| F-003 | expression-engine / evaluation | Function registry — name to callable, the plugin seam | planned | M2 | [detail](expression-engine/evaluation/F-003-function-registry.md) |
| F-004 | expression-engine / evaluation | Variables, assignment, and the constants `pi`, `e`, `ans` | planned | M2 | — |
| F-005 | expression-engine / evaluation | Built-in scientific functions: trig, log, exp, rounding | planned | M2 | — |
| F-006 | foundation / error-handling | Diagnostics with a caret pointing at the offending column | planned | M3 | — |
| F-007 | interface / terminal | One-shot mode: `ccalc "2+3"` evaluates, prints, exits | planned | M4 | — |
| F-008 | interface / terminal | REPL polish: history, line editing, `:help` `:vars` `:quit` | planned | M4 | — |
| F-009 | interface / session-state | Session history and `ans`, shared by both frontends | planned | M4 | — |
| F-010 | mathematics / precision | Arbitrary-precision integers and decimals | planned | M5 | — |
| F-011 | mathematics / matrix-handling | Matrices and linear algebra | planned | M5 | — |
| F-012 | mathematics / unit-handling | Unit conversion with dimensional analysis | planned | M5 | — |
| F-013 | graphing / geometry | 2D function graphing — sampling, scales, ticks, asymptotes | planned | M6 | [detail](graphing/geometry/F-013-2d-graphing.md) |
| F-014 | graphing / rendering | Terminal plot renderer using Unicode block characters | planned | M6 | — |
| F-015 | extensibility / plugin-system | Plugin ABI and host: discovery, loading, version checks | planned | M7 | [detail](extensibility/plugin-system/F-015-plugin-system.md) |
| F-016 | extensibility / plugin-system / first-party-plugins | Statistics plugin, dogfooding the ABI | planned | M7 | — |
| F-017 | interface / desktop | Desktop GUI over the same engine | planned | M8 | — |
| F-018 | interface / session-state | Save and restore workspaces | planned | M8 | — |

## Ideas — not committed

| ID | Topic path | Feature | Status | Notes |
|---|---|---|---|---|
| F-019 | foundation / value-model | Complex number support | idea | Needs `Value` to be a variant first (Q-004) |
| F-020 | graphing / geometry | Polar and parametric plots | idea | Natural plugin candidate once F-015 exists |

## Deliberately not doing

| ID | Topic path | Feature | Status | Reason |
|---|---|---|---|---|
| — | extensibility / plugin-system | Sandboxed plugins | dropped | Native code cannot be sandboxed without a much larger project than the calculator itself. `docs/PLUGINS.md` will say so plainly rather than imply safety that does not exist. See ADR 0006. |

## Milestones

| # | Theme | Features |
|---|---|---|
| M1 | Arithmetic | F-001, F-002 |
| M2 | Names | F-003, F-004, F-005 |
| M3 | Real errors | F-006 |
| M4 | REPL polish | F-007, F-008, F-009 |
| M5 | Mathematics | F-010, F-011, F-012 |
| M6 | Graphing | F-013, F-014 |
| M7 | Plugins | F-015, F-016 |
| M8 | GUI | F-017, F-018 |

**F-003, the registry, lands in M2 — five milestones before plugins.** That is
deliberate: a few hours of work, it makes the core cleaner regardless, and
everything in the plugin system attaches to it. See ADR 0006.
