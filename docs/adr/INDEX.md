# ADR index

Every decision record, newest first. **If it is not listed here, it does not
exist** — the index is the authority, not the folder tree.

Next free number: **0009**. Numbers are global and never reused.

| # | Topic path | Decision | Status |
|---|---|---|---|
| 0008 | project-structure / knowledge-base | [Six categories of nested topic folders](project-structure/knowledge-base/0008-knowledge-base-structure.md) | Accepted |
| 0007 | mathematics / unit-handling | [`units` belongs at L1, not L2](mathematics/unit-handling/0007-units-at-l1.md) | Accepted |
| 0006 | extensibility / plugin-system / abi | [The plugin boundary is a C ABI](extensibility/plugin-system/abi/0006-plugin-boundary-is-c.md) | Accepted |
| 0005 | graphing / geometry | [`plot` computes geometry and never renders](graphing/geometry/0005-plot-computes-geometry.md) | Accepted |
| 0004 | project-structure / module-layout | [Keep `include/` and `src/` separate per module](project-structure/module-layout/0004-separate-include-and-src.md) | Accepted |
| 0003 | foundation / error-handling | [Report errors with `std::expected`, not exceptions](foundation/error-handling/0003-expected-over-exceptions.md) | Accepted |
| 0002 | expression-engine / notation-handling | [Use a Pratt parser rather than shunting-yard](expression-engine/notation-handling/0002-pratt-parser.md) | Accepted |
| 0001 | project-structure / module-layout | [Split the code into layered modules under `libs/`](project-structure/module-layout/0001-modules-with-strict-layering.md) | Accepted |

## Status values

| Status | Meaning |
|---|---|
| `Proposed` | written, not yet agreed |
| `Accepted` | in force — the project follows this |
| `Superseded by NNNN` | replaced; kept for the record of what we used to think |

A superseded ADR is **never deleted or edited**, beyond adding its status line.
