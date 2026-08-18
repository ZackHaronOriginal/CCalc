# 7. units belongs at layer L1, not L2

**Status:** Accepted
**Date:** 2026-08-18

## Context

`units` was originally placed at L2, alongside `eval` and `functions`, on the
loose reasoning that unit handling is "semantic" work like evaluation.

Writing the module documentation exposed the problem. `eval` must be able to
compute `5 km + 3 m`, which means `eval` needs `units`. Both were at L2, so that
is a dependency between two modules in the same layer — exactly what ADR 0001
forbids, because sideways edges are where dependency cycles begin.

## Decision

`units` moves to **L1**, below `eval`.

This is honest rather than merely convenient: dimensional bookkeeping is
genuinely self-contained. Exponent vectors, conversion factors and lookup tables
need `core` and nothing else. In particular `units` does not need `numeric`, so
sitting in the same layer as it creates no problem.

## Alternatives considered

**Move `eval` up to L3.** Would also resolve the conflict, but `plot` at L3 needs
`eval`, so this just moves the same collision one layer up.

**Allow this one sideways dependency.** Rejected. The rule's entire value is that
it is not negotiable; the first exception makes the second one easy.

**Fold unit handling into `core`.** Rejected — it would put calculator-specific
knowledge into the module that is explicitly meant to contain none, and `core`
is already the most likely folder to decay into a junk drawer.

## Consequences

`eval` can handle united arithmetic directly. The layer table stays a strict DAG.

The generalisable lesson, worth more than this specific move: **when two modules
in the same layer need each other, one of them is in the wrong layer.** Moving
one down is the fix. Adding a sideways dependency is not.

Found before any code existed, so the cost was editing a table. The same mistake
discovered after both modules were written would have been a real refactor —
which is the argument for keeping the layer table written down and checking new
modules against it.
