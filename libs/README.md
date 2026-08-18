# libs/

**The calculator itself.** Every piece of real logic in CCalc lives in a module
under this directory. Nothing outside `libs/` contains anything worth testing.

## The two hard rules

**1. This directory is flat.** One level deep, always. There is no
`libs/math/numeric/`. When nesting is allowed, the tree quietly grows five levels
and nobody remembers whether statistics is under `math/stats/` or
`numeric/statistics/`. Flat means the full module list fits on one screen.

**2. There is no top-level `src/` or `include/`.** It is `libs/` or that, never
both. Having both means every new file poses the question "shared, or a module?",
and a question asked five hundred times gets answered inconsistently.

## The layer rule — the most important rule in the project

Every module sits in a layer. **A module may only depend on modules in a layer
strictly below it.** Never sideways, never upward.

| Layer | Modules | May depend on |
|---|---|---|
| L3 | `plot`, `session`, `plugin-host` | L2, L1, L0 |
| L2 | `eval`, `functions` | L1, L0 |
| L1 | `expr`, `numeric`, `units` | L0 |
| L0 | `core`, `plugin-abi` | nothing |

`apps/` sits above everything and may use any module. Nothing in `libs/` may
ever depend on anything in `apps/`.

### Why this is worth being strict about

Suppose `plot` uses `eval` to compute `f(x)` — fine, that points down. Later you
add `graph(sin(x))` as a typed command, and the quick fix is to let `eval` call
`plot`. Two lines, still compiles. But now neither module builds without the
other, your parser tests need the graphing system linked in, and every change to
either rebuilds both.

The fix — have `eval` return "this is a plot request" and let the app call
`plot` — costs twenty minutes on the day you notice, and weeks a year later.

**When a module needs something from a layer above it, that is a design signal,
not an inconvenience.** Invert the call so the higher layer does the work.

## What every module looks like inside

```
libs/<name>/
├── include/ccalc/<name>/   public headers — other modules may use these
├── src/                    sources and private headers — internal only
├── tests/                  this module's unit tests
├── CMakeLists.txt          added when the module gets its first real file
└── README.md               what this module is for
```

The build tells the compiler that other modules may look in `include/` and
nowhere else. So a private header in `src/` is not a polite suggestion — the
compiler physically stops another module from including it.

## Naming: one name, four places

| Folder | Build target | Namespace | Include path |
|---|---|---|---|
| `libs/expr/` | `ccalc_expr` | `ccalc::expr` | `<ccalc/expr/lexer.hpp>` |

Never break this mapping. Given any one of the four you can derive the other
three, which is how you navigate a large project without searching.

Always include with angle brackets and the full prefix:
`#include <ccalc/expr/lexer.hpp>`, never `#include "lexer.hpp"`. The prefix means
you can never accidentally pick up a different library's `lexer.hpp` — a nasty
bug, because it compiles and simply behaves wrong.

## When to create a new module

A new module is justified when the code has **its own reason to change** and a
name you can state in one sentence without using "and".

Do *not* create a module because a file is getting long — split the file. Do not
create one for code used by exactly one other module and nothing else — it
belongs inside that module.

## Don't create build files for empty modules

Every module folder exists here already, so the layout is decided and you will
not improvise a different one later. But a `CMakeLists.txt` for a module with no
code is pure maintenance for zero benefit. **Add the build file on the day the
module gets its first real source file.**
