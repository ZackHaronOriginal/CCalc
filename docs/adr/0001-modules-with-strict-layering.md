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
