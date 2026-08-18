# Open questions

Every undecided thing, one line each. **If it is not listed here, it is not
tracked.**

Next free ID: **Q-008**. IDs are never reused.

## Open

| ID | Topic | Question | Blocks | Decide by | Detail |
|---|---|---|---|---|---|
| Q-001 | gui | Qt or GTK for the desktop frontend? | F-017 | M8 — cheap to defer | [detail](gui/Q-001-toolkit-choice.md) |
| Q-002 | build | Catch2 or doctest for tests? | test setup | M0 — needed now | — |
| Q-003 | numeric | Own arbitrary-precision arithmetic, or GMP/MPFR? | F-010 | M5 — needs research on licensing | — |
| Q-004 | core | How does `Value` represent a united quantity? | F-012, F-019 | before M5 — expensive after | [detail](core/Q-004-value-and-units.md) |
| Q-005 | eval | Is angle mode global state, or part of a call? | F-005, F-013 | M2 | — |
| Q-006 | plugins | Must ABI v1 be able to pass arrays? | F-015, F-016 | **before ABI v1 ships — irreversible after** | [detail](plugins/Q-006-abi-arrays.md) |
| Q-007 | session | Workspace file format — JSON, TOML, or custom? | F-018 | M8 | — |

## Answered

*None yet.* Answered questions move here with their resolution and a link to
whatever settled them — an ADR, a research entry, or a one-line answer.

Format:

> **Q-000 · Should modules nest?** — No. [ADR 0001](../adr/project/0001-modules-with-strict-layering.md):
> nesting grows without limit and nobody remembers whether statistics lives under
> `math/stats/` or `numeric/statistics/`. `libs/` is flat.

---

## Reading the "Decide by" column

Not all deferral is equal, and the difference matters more than urgency:

- **Cheap to defer** — Q-001. ADR 0005 keeps `libs/plot` toolkit-free, so the
  choice stays reversible. Deferring costs nothing.
- **Needed soon** — Q-002, Q-005. Work is blocked, but a wrong answer is
  correctable.
- **Irreversible at a specific moment** — Q-006. Once ABI v1 is published, the
  contract is frozen forever. This is the one to watch.
