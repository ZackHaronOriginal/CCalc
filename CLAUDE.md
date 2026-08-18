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

## When you make a significant decision

Write an ADR in `docs/adr/`. Copy `0000-template.md`, give it the next number,
and record what was decided, what else was considered, and why. Decisions with
no recorded reasoning get re-litigated or silently reversed six months later.

## When you change something documented

Update the documentation in the **same commit**:

- grammar changes → `docs/GRAMMAR.md`
- plugin ABI changes → `docs/PLUGINS.md` **and** `plugins/example/`
- new module or layer change → `libs/README.md` **and** this file
