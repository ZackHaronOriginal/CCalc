# numeric

**Layer L1** · target `ccalc_numeric` · namespace `ccalc::numeric` · `<ccalc/numeric/…>`

Numbers and the mathematics done to them. **Knows nothing about expressions.**

## What goes here

- Arbitrary-precision integers and decimals
- Rational numbers
- Complex numbers
- `Matrix` and linear algebra — multiply, invert, determinant, solve
- Statistical kernels — mean, median, standard deviation, regression
- Numerical methods — root finding, numerical integration and differentiation
- Careful floating-point helpers: comparison with tolerance, safe conversion

## What does NOT go here

- **Anything about parsing or evaluating.** This module never sees an expression.
  It is a mathematics library that happens to live inside a calculator, and it
  should be usable in any other program unchanged.
- The user-facing function names. `stddev` as a *callable name the user types*
  belongs in `functions`; the algorithm that computes it belongs here.

## Why this separation is worth it

It keeps the hardest-to-get-right code — numerical algorithms — testable with
plain numbers and nothing else. A test here passes an array of doubles and checks
a result. No parser, no evaluator, no environment. When a statistics result is
wrong, you find out here, precisely.

## Rules

- Every algorithm here is unit tested against known values, including the
  awkward cases: empty input, one element, all-identical values, very large and
  very small magnitudes.
- Document the numerical behaviour in comments: what precision to expect, how it
  behaves near zero, when it loses accuracy. "It works" is not enough for
  floating-point code.
