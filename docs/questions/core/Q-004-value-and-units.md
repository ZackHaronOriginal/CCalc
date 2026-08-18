# Q-004. How does `Value` represent a united quantity?

**Topic:** `core` · **Status:** open
**Raised:** 2026-08-18 · **Blocks:** F-012, F-019
**Decide by:** before M5 — expensive to change afterwards

## The question

When the user types `5 km`, what does the resulting `Value` look like? Does the
unit live *inside* `Value` as another variant arm, or does `Value` carry an
optional dimension *alongside* its number?

## Why it matters

This decides whether **every** arithmetic operation must consider units, or only
the ones that opt in. It touches `core`, `eval`, `numeric` and `units` together,
which is why it must be settled before those modules are written rather than
after.

It also interacts with Q-019/F-019 (complex numbers): both are asking "what can a
`Value` be", and answering them separately risks two mechanisms that fight.

## Options

### Option A — a united quantity is a variant arm

`Value = variant<double, BigInt, Complex, Matrix, Quantity>` where `Quantity`
bundles a magnitude and a `Dimension`.

Clean type-wise, and the compiler forces every operation to handle it. But it
means unit handling appears in every arithmetic path, including ones that will
never see a unit, and the variant grows large.

### Option B — every `Value` carries an optional dimension

`Value = { number, optional<Dimension> }`. Unitless values simply have none.

Fewer variant arms and one uniform path. But every `Value` pays for a field most
never use, and "unitless" versus "dimensionless" becomes a distinction that has
to be handled deliberately rather than by the type system.

### Option C — units live one layer up

`Value` stays unit-free; `eval` tracks dimensions separately alongside values.

Keeps `core` simple, but the bookkeeping has to be threaded through evaluation by
hand, which is exactly the kind of thing that gets it wrong in one branch.

## What would settle it

Writing out the awkward cases and seeing which option handles them without
special-casing:

- `5 km + 3 m` → same dimension, different scale
- `5 km + 3 kg` → must error
- `5 km / 2 s` → produces a new dimension
- `sin(5 km)` → must error; trig needs dimensionless input
- `5 km * 2` → unit survives multiplication by a scalar
- `20 °C + 5 °C` → offset units, where naive factor conversion is wrong

That last one is the real test. Temperature has an offset, not just a factor, and
options that handle it awkwardly are probably wrong.

## Cost of deferring

**High and rising.** Every arithmetic operation written before this is settled
may have to be revisited. The `Value` type is the single most widely referenced
type in the project.

Deferring past M5 means retrofitting units into working code, which is
substantially more expensive than designing for them now.
