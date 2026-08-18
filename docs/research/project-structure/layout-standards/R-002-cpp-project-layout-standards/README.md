# R-002. What are the real standards for C++ project layout?

**Topic:** `project` · **Status:** complete
**Started:** 2026-08-18 · **Updated:** 2026-08-18
**Related:** ADR 0001, ADR 0004

## The question

C++ has no official project layout. Before committing CCalc to a structure meant
to last years, what conventions actually exist, and where do they disagree?

## The answer

Three sources matter, and **they agree on the big shape and disagree on one
specific point.**

1. **Pitchfork Layout (PFL)** — the closest thing to a de facto community
   standard. Defines `libs/` for splitting into submodules, with two hard rules:
   when `libs/` exists there is no top-level `src/` or `include/`, and submodules
   **cannot nest**. It also warns in its own words that submodules are *"an
   extremely heavy tool with subtleties that often trip people up."*

2. **P1204 "Canonical Project Structure"** (Kolpackov, WG21) — a paper submitted
   to the C++ committee. Argues headers should sit *beside* their sources, since
   editing a class means touching both, and notes that teams using the split
   layout put private headers in `include/` anyway, defeating its purpose. It also
   establishes the include-prefix rule: `#include <ccalc/expr/lexer.hpp>`.

3. **Godot** — a real, large, modular C++ application with a GUI. Its top level is
   a flat domain split: `core/ modules/ platform/ editor/ servers/ drivers/
   thirdparty/ tests/`. Notably there is **no `src/` and no `include/` at all**.

**The through-line:** at small scale, directories describe *what kind of file*
something is. At large scale, they describe *what part of the system* it is.
Godot is the proof that this is where real projects end up.

**The disagreement** is header placement, and it is genuine — both arguments are
sound. It is a trade-off, not a right answer.

## What this means for CCalc

- Adopt `libs/` with flat, non-nesting modules (PFL). → ADR 0001
- Adopt the `<project/module/file.hpp>` include prefix (P1204). → `libs/README.md`
- On headers, we chose PFL's separate `include/`+`src/` **against** P1204's
  advice, for two reasons P1204 could not weigh: with ten-plus modules the
  compiler-enforced public/private boundary is our main defence against
  uncontrolled coupling, and a plugin SDK needs exactly one directory of headers
  to publish. → ADR 0004
- Take PFL's warning seriously. Modules are heavy — which is why we create the
  folders now but add build targets only when a module gets real code.

## Detail

**PFL's prescribed top-level directories:** `build/`, `include/`, `src/`,
`tests/`, `examples/`, `external/`, `data/`, `tools/`, `docs/`, `libs/`,
`extras/`. It also distinguishes `libs/` (built by default) from `extras/`
(optional add-ons).

**P1204's naming scheme:** `.hpp` headers, `.cpp` sources, `.mpp` module
interfaces, `.test.cpp` unit tests; `lib`-prefixed library names; macros,
namespaces and modules all qualified by project name.

**On nesting:** PFL forbids sub-submodules outright. Our own reasoning agrees —
once nesting is allowed nobody remembers whether statistics is under
`math/stats/` or `numeric/statistics/`, and a flat list stays scannable.

## Rejected

**A single `src/` + `include/`** (what most small C++ projects use). Fine below
roughly 5,000 lines. Rejected because it offers no mechanism for controlling
coupling, and by the time that hurts the fix costs weeks.

**Nested modules.** Rejected on PFL's advice and our own reasoning above.

**Following P1204 on header placement.** Rejected with real reluctance — the
navigation argument is correct and we now pay that cost daily. See ADR 0004 for
the full weighing.

**Copying Godot's exact layout.** Its structure is shaped by being a game engine:
`servers/`, `scene/`, `drivers/` have no counterpart here. The *principle* —
top-level directories as domains — transfers; the specific names do not.

## Confidence and gaps

**Solid.** All three sources were read directly, and PFL's rules and P1204's
argument are quoted rather than paraphrased from memory.

**Judgement, not fact.** The choice between PFL and P1204 on headers is a
trade-off we resolved for our circumstances. A different project could reasonably
choose the other way, and ADR 0004 records the reasoning so it can be revisited
if those circumstances change.

**Not surveyed.** We did not examine other large C++ applications (Blender, Qt,
LLVM) in detail. Godot alone may not be representative — though it agreeing with
PFL's direction is a useful signal.
