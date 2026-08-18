# Architecture Decision Records

One short file per significant decision: what was decided, what else was
considered, and **why**.

## Why bother

Six months from now, someone — possibly you — will look at a choice here and
think "that seems arbitrary, let me simplify it." Without a written reason, they
will either re-litigate a settled question or quietly reverse it and rediscover
the original problem the hard way.

An ADR is insurance against that. It costs ten minutes to write.

## How to add one

1. Copy `0000-template.md`
2. Name it `NNNN-short-title.md` with the next free number
3. Fill it in and commit it with the change it describes

## Rules

- **Never delete or rewrite an accepted ADR.** If a decision is reversed, write a
  *new* one that supersedes it, and mark the old one `Superseded by NNNN`. The
  record of what we used to think, and why we changed, is the valuable part.
- Keep them short. A page is plenty.
- Record the alternatives you rejected. An ADR without them only says what we do,
  not why the obvious other option is worse — and the obvious other option is
  exactly what someone will propose later.

## What deserves an ADR

Anything hard to reverse, or anything where a reasonable person would choose
differently: module boundaries and layers, the plugin ABI, dependency choices,
file format decisions, the UI toolkit.

Not: naming a variable, splitting a long function, ordinary refactors.

## Index

| # | Decision | Status |
|---|---|---|
| [0001](0001-modules-with-strict-layering.md) | Layered modules under `libs/` | Accepted |
| [0002](0002-pratt-parser.md) | Pratt parser over shunting-yard | Accepted |
| [0003](0003-expected-over-exceptions.md) | `std::expected` over exceptions | Accepted |
| [0004](0004-separate-include-and-src.md) | Separate `include/` and `src/` per module | Accepted |
| [0005](0005-plot-computes-geometry.md) | `plot` computes geometry, never renders | Accepted |
| [0006](0006-plugin-boundary-is-c.md) | The plugin boundary is a C ABI | Accepted |
| [0007](0007-units-at-l1.md) | `units` sits at L1, not L2 | Accepted |
