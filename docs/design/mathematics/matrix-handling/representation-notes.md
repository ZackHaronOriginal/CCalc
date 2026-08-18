# Matrix representation notes

**Topic:** mathematics / matrix-handling · **Status:** draft
**Related:** F-011, Q-004, `libs/numeric/`

Early thinking on how matrices are stored and how they reach the user.

## Storage

Row-major, in a single contiguous `std::vector<double>` with stored dimensions —
not a vector of vectors. Contiguous storage keeps cache behaviour predictable and
makes it trivial to hand a pointer plus a length across the plugin boundary,
which matters for ADR 0006 and Q-006.

Small-matrix optimisation is worth considering later. Calculator use is dominated
by 2×2 and 3×3, and a heap allocation for nine doubles is wasteful — but that is
an optimisation to measure, not to assume.

## What "matrix" means to the user

A matrix is a `Value`, so it flows through evaluation like any number. That means
every operator needs a defined behaviour:

| Expression | Result |
|---|---|
| `A + B` | element-wise; error if shapes differ |
| `A * B` | matrix product; error if inner dimensions disagree |
| `A * 2` | scalar multiply |
| `A / B` | **undecided** — `A * inv(B)`, or an error, or element-wise? |
| `A ^ 2` | repeated multiplication; requires square |
| `A ^ -1` | inverse, or an error? |

The `A / B` row is a genuine trap. Division is not defined for matrices, and
silently choosing `A * inv(B)` gives a confident answer to a question the user
may not have meant to ask. Erroring and requiring `inv()` explicitly is probably
right, and is the kind of thing that deserves an ADR rather than a quiet choice
in an implementation.

## Interaction with Q-004

If `Value` gains a dimension field for units, matrices inherit the question: does
a matrix carry one dimension for all elements, or one per element? One per matrix
is far simpler and covers nearly every real use. Worth settling as part of Q-004
rather than separately.

## Errors to get right

Shape mismatch is the common user error, and the message should say both shapes:
`cannot add 2x3 and 3x2` — not `dimension mismatch`. Singular matrices need
detection with a tolerance, not an exact zero-determinant test.
