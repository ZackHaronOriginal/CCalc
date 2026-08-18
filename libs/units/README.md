# units

**Layer L1** · target `ccalc_units` · namespace `ccalc::units` · `<ccalc/units/…>`

Physical units, conversion, and dimensional analysis.

## What goes here

- `Dimension` — the exponents of the seven SI base dimensions
  (length, mass, time, current, temperature, amount, luminous intensity)
- `Unit` — a name, a dimension, a conversion factor, and an offset for the
  awkward ones like Celsius
- Unit tables: SI, imperial, and the domain-specific ones people expect
- Conversion, and the arithmetic rules: adding two lengths is fine, adding a
  length to a mass is an error, dividing a length by a time yields a velocity

## What does NOT go here

- Currency conversion. Rates change daily and need a network fetch — that is a
  plugin, not a core module.
- Parsing the syntax `5 km -> miles`. The grammar belongs in `expr`; this module
  only answers what a unit *is* and how to convert it.
- Heavy numerics. This module works in plain `double` magnitudes and dimension
  exponents. If something here needs matrices or arbitrary precision, it is in
  the wrong module.

## Why this sits at L1 and not L2

It was originally placed at L2 next to `eval`, and that was wrong. `eval` has to
know how to add `5 km + 3 m`, so it needs this module — and a dependency between
two modules in the same layer is exactly what the layer rule forbids.

The fix is that `units` belongs *below* `eval`, not beside it. That works because
dimensional bookkeeping is genuinely self-contained: exponent vectors,
conversion factors and lookup tables need `core` and nothing else. It does not
need `numeric`, so L1 is an honest placement rather than a convenient one.

This is worth remembering as an example: when two modules in the same layer need
each other, one of them is usually in the wrong layer. Moving it down is the fix.
Adding a sideways dependency is not.

## Why dimensional analysis is worth the effort

It turns a whole category of silent wrong answers into clear errors. A user who
accidentally adds a time to a distance gets told so, instead of receiving a
confident, meaningless number. That is a real advantage over the simple
calculators CCalc is meant to beat.
