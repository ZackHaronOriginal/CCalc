# ADR index

Every decision record, newest first. **If it is not listed here, it does not
exist** — the index is the authority, not the folder listing.

The next free number is **0008**. Numbers are global and never reused, even
across topics: "ADR 0004" must mean exactly one document forever.

| # | Topic | Decision | Status |
|---|---|---|---|
| 0007 | units | [`units` belongs at L1, not L2](units/0007-units-at-l1.md) | Accepted |
| 0006 | plugins | [The plugin boundary is a C ABI](plugins/0006-plugin-boundary-is-c.md) | Accepted |
| 0005 | plot | [`plot` computes geometry and never renders](plot/0005-plot-computes-geometry.md) | Accepted |
| 0004 | project | [Keep `include/` and `src/` separate per module](project/0004-separate-include-and-src.md) | Accepted |
| 0003 | core | [Report errors with `std::expected`, not exceptions](core/0003-expected-over-exceptions.md) | Accepted |
| 0002 | expr | [Use a Pratt parser rather than shunting-yard](expr/0002-pratt-parser.md) | Accepted |
| 0001 | project | [Split the code into layered modules under `libs/`](project/0001-modules-with-strict-layering.md) | Accepted |

## Status values

| Status | Meaning |
|---|---|
| `Proposed` | written, not yet agreed |
| `Accepted` | in force — the project follows this |
| `Superseded by NNNN` | replaced; kept for the record of what we used to think |

A superseded ADR is **never deleted or edited**, beyond adding its status line.
The record of what we believed and why we changed is the valuable part.
