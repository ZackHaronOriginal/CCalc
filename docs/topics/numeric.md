# numeric

**Layer:** L1 · **Module:** [`libs/numeric/`](../../libs/numeric/README.md)

Numbers and the mathematics performed on them: arbitrary precision, rationals,
complex numbers, matrices, linear algebra, statistics, numerical methods.

## In scope

Algorithms and their correctness. Floating-point behaviour, precision loss,
tolerance, overflow. Anything that would still be true in a program that was not
a calculator.

## Not in scope

**User-facing names.** `stddev` as a name a person types is `functions`; the
algorithm computing it is here. Also not in scope: parsing, evaluation, or any
awareness that expressions exist.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `functions` | The algorithm → `numeric`. The callable name and its arity → `functions` |
| `core` | `core` defines what a `Value` is; `numeric` computes with numbers |
| `units` | Dimensional bookkeeping → `units`. Arithmetic on magnitudes → `numeric` |

## Watch out

This is the hardest topic to get right and the easiest to get *subtly* wrong. A
statistics function that is off in the fourth decimal will not crash and nobody
will notice. Everything here needs tests against known values, including empty
input, single elements, and extreme magnitudes.
