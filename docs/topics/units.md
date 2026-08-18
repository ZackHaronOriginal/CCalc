# units

**Layer:** L1 · **Module:** [`libs/units/`](../../libs/units/README.md)

Physical units, conversion, and dimensional analysis.

## In scope

`Dimension` as exponents over the seven SI base dimensions. `Unit` as a name,
dimension, factor and offset. The unit tables. The arithmetic rules: two lengths
add, length plus mass is an error, length over time is a velocity.

## Not in scope

**Currency** — rates change daily and need a network fetch, which makes it a
plugin, not a core module. **The syntax** `5 km -> miles` — that grammar is
`expr`; this topic only answers what a unit is and how to convert it.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `numeric` | Dimension tracking → `units`. Arithmetic on the magnitude → `numeric` |
| `eval` | Applying unit rules during evaluation is `eval` calling `units` |

## Decisions in force

ADR 0007 — sits at **L1, not L2**. It was originally placed beside `eval`, until
it became clear `eval` must handle `5 km + 3 m` and would be depending on its own
layer. The general lesson: when two modules in one layer need each other, one is
in the wrong layer.

## Open

Q-004 — how a united quantity is represented in `Value`. Settle before M5;
changing it later touches every arithmetic operation.
