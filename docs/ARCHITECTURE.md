# CCalc — Architecture

Status: **planning**. The folder structure exists; the implementation does not.
This document records the decisions the layout encodes and the reasoning behind
them. Each folder's own README says what belongs inside it.

---

## 1. What CCalc is

A terminal calculator, growing into a scientific and graphing calculator with a
desktop frontend and a plugin system.

```
> 2 + 3 * 4
14
> x = 5
5
> sqrt(x^2 + 12^2)
13
> 5 km + 300 m -> miles
3.28796
```

Two frontends, one engine: a CLI (REPL plus a scriptable one-shot mode) and,
later, a GUI. Both call the same library code.

---

## 2. How an expression is processed

```
"2 + 3 * 4"
     │
     ▼
┌──────────┐  tokens   ┌──────────┐   AST    ┌───────────┐  Value
│  Lexer   │ ────────▶ │  Parser  │ ───────▶ │ Evaluator │ ──────▶ output
└──────────┘           └──────────┘          └───────────┘   14
   expr                    expr                   eval
                                                   │
                                          ┌────────┴────────┐
                                          ▼                 ▼
                                     Environment        Registry
                                     (variables)      (functions)
                                        eval            functions
```

The **registry** is the seam the plugin system attaches to. `eval` asks it for a
name and calls whatever comes back, with no knowledge of where it came from.

---

## 3. Module layers

A module may depend only on modules in a layer **strictly below** it. Never
sideways, never upward.

| Layer | Modules | Purpose |
|---|---|---|
| L3 | `plot`, `session`, `plugin-host` | orchestration, persistence, loading |
| L2 | `eval`, `functions` | meaning: evaluation and the function registry |
| L1 | `expr`, `numeric`, `units` | syntax, mathematics, dimensions |
| L0 | `core`, `plugin-abi` | shared types, and the frozen plugin contract |

`apps/` sits above all of it and may use anything. Nothing in `libs/` may ever
depend on `apps/`.

**Why strictness pays.** If `eval` ever calls `plot` — the natural way to
implement a typed `graph(sin(x))` command — neither module can be built or
tested alone again, and every change to either rebuilds both. The fix, having
`eval` return a plot *request* the app acts on, costs twenty minutes on the day
you notice and weeks a year later.

**Corollary.** When two modules in the same layer need each other, one of them
is in the wrong layer. `units` started at L2 beside `eval`, but `eval` must
handle `5 km + 3 m`, so `units` moved down to L1. Moving a module down is the
fix; a sideways dependency is not.

---

## 4. Decisions and their reasoning

| Decision | Choice | Why |
|---|---|---|
| Top-level layout | `libs/` of flat modules, no top-level `src/` | Work happens one *feature* at a time, not one file-kind at a time |
| Parser | Pratt (precedence climbing) | Handles unary minus, right-associative `^` and calls naturally; shunting-yard does not |
| Errors | `std::expected<T, Diagnostic>` | Bad input is the normal case in a calculator, not an exceptional one |
| Value type | `struct Value` wrapping a `double` | Becomes a `std::variant` later without touching call sites |
| Headers | `include/` and `src/` separate, per module | The compiler enforces the public/private boundary; also gives plugin authors a single SDK directory |
| Graphing | Geometry only; frontends render | Keeps the hard maths testable and the toolkit choice reversible |
| Tests | Unit inside the module, integration at root | A failing unit test names the guilty module immediately |
| Plugins | C ABI, header-only L0 contract, registry seam | C++ has no stable ABI; a minimal contract is the smallest promise we must keep forever |

---

## 5. Build order

Each milestone ends with something that runs.

| # | Milestone | Contents | Done when |
|---|---|---|---|
| M0 | Skeleton | Folder layout, CMake split, test framework, one passing test | `cmake --build` and `ctest` both green |
| M1 | Arithmetic | `core`, `expr`, `eval`, `apps/cli`. `+ - * / % ^`, parens, unary minus | `ccalc "2+3*4"` prints `14` |
| M2 | Names | `functions` **including the registry**, variables, constants | `x = 5` then `sqrt(x^2+144)` → `13` |
| M3 | Real errors | `Diagnostic` plumbed through, caret output, no crash on any input | Every bad input gets a pointed message |
| M4 | REPL polish | `session`, `ans`, `:help` / `:vars` / `:quit`, history, one-shot mode | Feels like a tool, not a demo |
| M5 | Maths | `numeric`, `units` | Matrices, big numbers, unit conversion |
| M6 | Graphing | `plot` geometry, terminal renderer in `apps/cli` | A plot appears in the terminal |
| M7 | Plugins | `plugin-abi`, `plugin-host`, `plugins/example`, `plugins/stats` | A `.so` dropped in the plugin directory adds a working function |
| M8 | GUI | `apps/gui` | Same engine, graphical frontend |

The registry lands in **M2**, long before plugins in M7. That is deliberate: it
is a few hours of work, it makes the core cleaner regardless, and everything in
the plugin system attaches to it.

---

## 6. What is deliberately deferred

User-defined functions · symbolic algebra · complex-number display formats ·
network features · a plugin sandbox.

The `Value`, `Environment` and registry designs leave room for the first three.
The last one is out of scope: a native plugin runs as ordinary code inside our
process, and `docs/PLUGINS.md` will say so plainly rather than implying a safety
that does not exist.

---

## 7. Publishing the plugin ABI

The expensive, irreversible step is not writing the loader — it is **publishing
the contract**. Once an outside plugin ships, `libs/plugin-abi/` can never change
incompatibly again.

So: design for it from day one, publish it last. Build the registry in M2, keep
`plugin-abi` honest by routing our own built-ins through it, write our own
plugins first to find the awkward parts, and declare ABI version 1 only when we
are ready to keep that promise.
