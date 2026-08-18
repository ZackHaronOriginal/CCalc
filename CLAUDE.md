# CCalc — project context

A terminal calculator for Linux, growing into a scientific and graphing
calculator with a desktop frontend and a plugin system. C++23, CMake.

**Status: early.** The structure exists; the calculator does not. `apps/cli/main.cpp`
is still a Hello World stub.

---

## Read these before changing anything

- `libs/README.md` — the layer rule, the naming rule, module conventions
- `docs/ARCHITECTURE.md` — decisions and reasoning
- `docs/adr/` — why each significant choice was made
- Every directory has a README stating what belongs in it and what does not.
  **Read the README of any directory before adding a file to it.**

---

## The rules, in priority order

**1. Dependencies point down.** Modules are layered. A module may depend only on
modules in a layer strictly below it — never sideways, never upward.

| Layer | Modules |
|---|---|
| L3 | `plot`, `session`, `plugin-host` |
| L2 | `eval`, `functions` |
| L1 | `expr`, `numeric`, `units` |
| L0 | `core`, `plugin-abi` |

`apps/` sits above everything. Nothing in `libs/` may depend on `apps/`.

When two modules in the same layer seem to need each other, **one of them is in
the wrong layer.** Move it down. Do not add a sideways dependency, and do not
"temporarily" break the rule.

**2. Apps contain no logic.** If it is worth testing, it belongs in `libs/`.
`apps/` does argument parsing, I/O, and wiring — nothing else.

**3. `plot` computes geometry, never renders.** No UI toolkit header may appear
anywhere in `libs/`. Not once.

**4. The plugin boundary is plain C.** No C++ types cross it, no exception
escapes it, `abi_version` is the first struct field. See `libs/plugin-abi/README.md`
before touching that module — its headers are a permanent promise.

**5. `libs/` is flat.** One level deep. No `libs/math/numeric/`.

---

## Naming — one name, four places

| Folder | Target | Namespace | Include |
|---|---|---|---|
| `libs/expr/` | `ccalc_expr` | `ccalc::expr` | `<ccalc/expr/lexer.hpp>` |

Never break this mapping. Always angle brackets with the full `ccalc/` prefix,
never `#include "lexer.hpp"`.

## Module shape

```
libs/<name>/
├── include/ccalc/<name>/   public headers (other modules may use)
├── src/                    sources + private headers (internal only)
├── tests/                  unit tests for this module
└── CMakeLists.txt          added when the module gets its first source file
```

Do not create a `CMakeLists.txt` for a module that has no code yet.

---

## Conventions

- Errors use `std::expected<T, Diagnostic>`, not exceptions. Bad input is the
  normal case in a calculator.
- Files `snake_case.cpp/.hpp`; types `PascalCase`; functions and variables
  `snake_case`.
- Unit tests live in the module (`libs/<name>/tests/`). Root `tests/` is for
  cross-module tests only.
- Never skip or disable a test to get a green build.

## Build

```sh
cmake -B build
cmake --build build
./build/CCalc
```

---

---

## The knowledge base — where your work goes

`docs/` has a reference half and a knowledge-base half. **Findings, decisions,
plans and defects all belong in the knowledge base, not in a chat reply that
disappears.**

| You did this | It goes here | Index to update |
|---|---|---|
| Researched a topic | `docs/research/<topic>/R-NNN-name/README.md` | `docs/research/INDEX.md` |
| Made a significant decision | `docs/adr/<topic>/NNNN-name.md` | `docs/adr/INDEX.md` |
| Identified work to do | a row in `docs/features/INDEX.md` | — |
| Found a defect | a row in `docs/bugs/INDEX.md` | — |
| Hit something undecided | a section in `docs/QUESTIONS.md` | — |

Topics come from `docs/TOPICS.md`. **Use an existing topic** — inventing
`parser` next to `expr` destroys the value of a shared list.

### Four rules, no exceptions

1. **The index is the authority.** An item not in its `INDEX.md` does not exist.
   Always update the index in the same commit as the item.
2. **One line until it earns more.** Start every item as a single row. Give it a
   file only when there is something that will not fit on the line, and a folder
   only when it has several artifacts. **Do not create empty template files** —
   they look like documentation and contain nothing.
3. **Never delete.** Dropped features, fixed bugs, superseded research and
   answered questions stay, with a terminal status.
4. **Never reuse an ID.** Each index records the next free one.

### Writing research

The `README.md` of a research entry is the deliverable and must be readable on
its own. **State the conclusion first** — a file that buries the answer below the
methodology will not be read. Then: what it means for CCalc, what was rejected
and why, and an honest confidence section distinguishing "verified by building
it" from "the documentation claims this". Record sources in `sources.md`.

Research that concludes nothing and changes nothing is not finished.

---

## When you change something documented

Update the documentation in the **same commit**:

- grammar changes → `docs/GRAMMAR.md`
- plugin ABI changes → `docs/PLUGINS.md` **and** `plugins/example/`
- new module or layer change → `libs/README.md` **and** this file
- a settled question → move it to Answered in `docs/QUESTIONS.md` and link what
  settled it
