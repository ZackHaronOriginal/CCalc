# CCalc — Architecture & Project Plan

Status: **planning**. Nothing here is implemented yet; this document is the target
we build toward. It defines the folder layout, the pipeline an expression travels
through, and the order we build things in.

---

## 1. What CCalc is

A terminal calculator. You type an expression, it prints the result:

```
> 2 + 3 * 4
14
> x = 5
5
> sqrt(x^2 + 12^2)
13
> ans / 2
6.5
```

Two ways to run it:

- **REPL mode** — `ccalc` with no arguments, reads lines until EOF or `:quit`.
- **One-shot mode** — `ccalc "2 + 3 * 4"` evaluates, prints, exits. Makes it
  scriptable and makes end-to-end testing trivial.

---

## 2. The core structural decision: library + thin executable

The single most important choice up front. Everything that *is* the calculator
lives in a static library, `ccalc_core`. The executable `ccalc` is a ~20-line
`main.cpp` that parses argv and calls into the library.

Why this matters:

- **Tests link the library directly.** If the logic lives in the executable, unit
  tests can only poke at it through stdin/stdout. With a library, a test calls
  `evaluate("2+3")` and checks it returns `5`. This is the difference between
  tests that are pleasant to write and tests you avoid writing.
- The I/O layer (prompt, history, colors) stays cleanly separated from the
  language layer (lex, parse, eval).
- If a GUI or a WASM build ever happens, the core is already reusable.

---

## 3. Folder structure

```
CCalc/
├── CMakeLists.txt              # top-level: options, standard, add_subdirectory
├── README.md
├── LICENSE
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml              # build + test on Linux/macOS/Windows
├── cmake/                      # helper modules, added only when needed
├── docs/
│   ├── ARCHITECTURE.md         # this file
│   └── GRAMMAR.md              # the formal grammar, kept in sync with parser
├── include/
│   └── ccalc/                  # public headers — the library's API surface
│       ├── token.hpp
│       ├── lexer.hpp
│       ├── ast.hpp
│       ├── parser.hpp
│       ├── value.hpp
│       ├── environment.hpp
│       ├── evaluator.hpp
│       ├── diagnostic.hpp
│       └── repl.hpp
├── src/
│   ├── CMakeLists.txt
│   ├── main.cpp                # thin entry point: argv → repl or one-shot
│   └── ccalc/                  # library implementation
│       ├── lexer.cpp
│       ├── parser.cpp
│       ├── evaluator.cpp
│       ├── environment.cpp
│       ├── builtins.cpp
│       ├── diagnostic.cpp
│       └── repl.cpp
└── tests/
    ├── CMakeLists.txt
    ├── test_lexer.cpp
    ├── test_parser.cpp
    ├── test_evaluator.cpp
    └── test_end_to_end.cpp
```

Conventions:

- Headers live under `include/ccalc/` and are included as `#include <ccalc/lexer.hpp>`.
  The `ccalc/` subdirectory prevents header name collisions and makes includes
  self-documenting.
- Everything in the library sits in `namespace ccalc`.
- Files are `snake_case.cpp/.hpp`, types are `PascalCase`, functions and variables
  are `snake_case`. Pick one and never think about it again.

---

## 4. How the calculator actually works

One line of input travels through four stages:

```
"2 + 3 * 4"
     │
     ▼
┌──────────┐   tokens          ┌──────────┐   AST             ┌───────────┐   Value
│  Lexer   │ ────────────────▶ │  Parser  │ ────────────────▶ │ Evaluator │ ────────▶ output
└──────────┘  NUM(2) PLUS      └──────────┘   Binary(+,       └───────────┘   14
              NUM(3) STAR                       2,
              NUM(4) EOF                        Binary(*,3,4))      │
                                                                    ▼
                                                            ┌──────────────┐
                                                            │ Environment  │
                                                            │ vars + funcs │
                                                            └──────────────┘
```

### 4.1 Lexer (`lexer.hpp` / `lexer.cpp`)

Turns the raw string into a flat sequence of `Token`s. A token is a kind
(`Number`, `Identifier`, `Plus`, `Star`, `LParen`, …), the source text it came
from, and its column offset. That offset is what lets us later print:

```
> 2 + * 4
      ^ expected a number, found '*'
```

The lexer is a straight character loop — no regex, no lookahead beyond one
character. It is the easiest part to write and the easiest to test exhaustively.

### 4.2 Parser (`parser.hpp` / `parser.cpp`)

Tokens in, AST out. **Recommendation: a Pratt parser** (precedence-climbing).

Why Pratt over shunting-yard: shunting-yard produces a flat RPN stream and gets
awkward the moment you add unary minus, right-associative `^`, or function calls.
A Pratt parser handles all three naturally, produces a real tree, and stays about
the same size. It's ~120 lines and each operator's precedence is one table entry,
so adding an operator later is a one-line change.

The AST node types, kept deliberately small:

| Node       | Holds                                | Example      |
|------------|--------------------------------------|--------------|
| `Number`   | `double`                             | `3.14`       |
| `Variable` | name                                 | `x`          |
| `Unary`    | op, operand                          | `-x`         |
| `Binary`   | op, left, right                      | `a + b`      |
| `Call`     | callee name, argument list           | `sqrt(2)`    |
| `Assign`   | name, value expression               | `x = 5`      |

Nodes are held as `std::unique_ptr<Expr>` in a variant-based or virtual hierarchy —
either works; the tree is small and short-lived, so clarity beats cleverness.

### 4.3 Evaluator (`evaluator.hpp` / `evaluator.cpp`)

Walks the tree and folds it to a `Value`. A recursive post-order traversal:
evaluate children, apply the operator. Consults the `Environment` for variable
lookups and function calls.

### 4.4 Environment (`environment.hpp`)

A `std::unordered_map<std::string, Value>` for variables plus a registry of
built-in functions. Pre-seeded with `pi`, `e`, and `ans` (the previous result).
Built-ins to start: `sqrt abs min max floor ceil round sin cos tan ln log pow`.

---

## 5. Two design questions worth settling now

### 5.1 What is a `Value`?

**Recommendation: start with `double`, but wrap it in a `struct Value`** rather
than passing bare doubles around. Costs nothing today, and when integers,
booleans (for comparison operators), or rationals show up later, `Value` becomes
a `std::variant` without touching a single call site.

### 5.2 How do errors travel?

**Recommendation: `std::expected<T, Diagnostic>` (C++23), not exceptions.**

In a calculator, malformed input is *the normal case* — the user typos constantly.
Exceptions are for the exceptional, and paying stack-unwinding cost plus
`try`/`catch` noise on every single REPL line is the wrong shape. `std::expected`
makes "this might fail" visible in every signature, and we already target C++23
so it's free.

A `Diagnostic` carries a message, a column, and a length, so the REPL can print
the caret line shown in §4.1.

---

## 6. Build layout

Top-level `CMakeLists.txt`:

- `cmake_minimum_required(VERSION 3.20)` — **lower this from the current 4.1**.
  CMake 4.1 is very new and shuts out most installed toolchains for no benefit;
  nothing here needs it.
- `set(CMAKE_CXX_STANDARD 23)` + `CXX_STANDARD_REQUIRED ON`
- `option(CCALC_BUILD_TESTS "Build tests" ON)`
- `add_subdirectory(src)`, and `tests` only when the option is on

`src/CMakeLists.txt` defines `ccalc_core` (static lib, `target_include_directories`
pointing at `include/`) and `ccalc` (executable, links `ccalc_core`).

**Tests: Catch2 v3 via `FetchContent`.** No system install needed, `ctest`
integration is one line. doctest is the lighter alternative if compile times ever
become annoying — the test bodies look nearly identical either way, so switching
later is cheap.

---

## 7. Build order

Each milestone ends with something runnable. Nothing is built that isn't used by
the next step.

| # | Milestone | Contents | Done when |
|---|-----------|----------|-----------|
| **M0** | Skeleton | Folder layout, CMake split, Catch2 wired, one trivial passing test | `cmake --build` + `ctest` both green |
| **M1** | Arithmetic | Lexer, Pratt parser, evaluator. `+ - * / % ^`, parens, unary minus, float literals | `ccalc "2+3*4"` prints `14` |
| **M2** | Names | `Environment`, variables, assignment, constants, built-in functions | `x = 5` then `sqrt(x^2+144)` → `13` |
| **M3** | Real errors | `Diagnostic`, `std::expected` plumbed through, caret error output, div-by-zero and unknown-name handling | Every bad input gets a pointed message, never a crash |
| **M4** | REPL polish | `ans`, `:help` / `:vars` / `:quit`, line history, one-shot argv mode | Feels like a tool, not a demo |
| **M5** | CI | GitHub Actions matrix, warnings-as-errors, `ctest` on every push | Badge is green |

Deliberately deferred: comparison operators, user-defined functions, integer/
rational types, unit conversion. The `Value` and `Environment` designs above leave
room for all of them.

---

## 8. Testing approach

- **Lexer tests** — string in, expected token sequence out. Cheap and exhaustive.
- **Parser tests** — parse, then compare against a canonical parenthesized
  rendering of the tree. `"2+3*4"` → `"(2 + (3 * 4))"`. One string comparison
  proves precedence and associativity, which is where parser bugs actually live.
- **Evaluator tests** — expression in, numeric result out, with a tolerance for
  floating point.
- **End-to-end tests** — drive the one-shot mode, assert on stdout and exit code.

Error cases get equal weight to happy paths: unbalanced parens, trailing
operators, unknown identifiers, wrong argument counts, division by zero.
