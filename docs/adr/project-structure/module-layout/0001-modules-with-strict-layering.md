# 1. Split the code into layered modules under libs/

**Status:** Accepted
**Date:** 2026-08-18

## Context

CCalc is planned to grow well beyond a simple calculator: scientific functions,
graphing, unit conversion, two frontends, and a plugin system. A single `src/`
directory works for twenty files and becomes unnavigable at four hundred.

The deeper problem is not navigation but coupling. Without a structural rule,
dependencies accumulate in every direction until no part can be built or tested
without all the others.

## Decision

All logic lives in modules under `libs/`, one directory per module, **flat** —
one level deep, no nesting. There is no top-level `src/` or `include/`.

Each module belongs to a layer, and **may depend only on modules in a layer
strictly below it.** Never sideways, never upward.

| Layer | Modules |
|---|---|
| L3 | `plot`, `session`, `plugin-host` |
| L2 | `eval`, `functions` |
| L1 | `expr`, `numeric`, `units` |
| L0 | `core`, `plugin-abi` |

## Alternatives considered

**Single `src/` + `include/`.** Simpler, and what most small C++ projects do.
Rejected because it gives no mechanism at all for controlling coupling, and by
the time that hurts, the fix costs weeks.

**Nested modules** (`libs/math/numeric/`). Rejected on Pitchfork's advice and
our own: nesting grows without limit, and nobody remembers whether statistics is
under `math/stats/` or `numeric/statistics/`. Flat keeps the module list on one
screen.

**Layers as a guideline rather than a rule.** Rejected. A guideline is broken by
the first convenient two-line shortcut, and a dependency cycle is far cheaper to
prevent than to remove.

## Consequences

Each module builds and tests alone, so changing the plotter does not rebuild the
parser. The dependency graph stays a DAG. The plugin system becomes possible
without disturbing lower layers.

The costs are real: more `CMakeLists.txt` files, deeper include paths, and
genuine up-front thought about what the modules are. Moving code between modules
later is annoying enough that the initial split matters.

The rule will be tested by exactly one situation: `graph(sin(x))` typed as a
command, where the natural implementation has `eval` (L2) call `plot` (L3). The
answer is that `eval` returns a plot *request* and the app acts on it. Recorded
here because that is the moment the rule will feel inconvenient.

---

## Scope clarification

*Added 2026-08-18. Clarifies scope; does not change the decision.*

**The no-nesting rule applies to `libs/` only** — to compiled code modules. It
does **not** apply to the documentation tree under `docs/`, which nests freely by
design (ADR 0008).

The two are not in conflict because the thing that makes nesting harmful in
`libs/` is absent in `docs/`:

| | `libs/<module>/` | `docs/<category>/<topic path>/` |
|---|---|---|
| Has a build target | yes — one per module | no |
| Has an identity elsewhere | yes — namespace, include path, target name, all mirroring the folder | no |
| Cost of nesting | the four-way name mapping breaks; `libs/math/numeric/` has no obvious target name | none |
| Cost of *not* nesting | none | notes on one subject pile into one flat folder with nowhere to subdivide |
| How you find things | the flat module list, visible on one screen | the category's `INDEX.md` |

A module is a *thing the build knows about*, and its folder path is one of four
places its name appears. A note is just a file. Nesting a module creates real
ambiguity; nesting a note creates useful structure.
