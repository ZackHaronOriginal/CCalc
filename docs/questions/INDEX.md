# Open questions

Every undecided thing, one line each. **If it is not listed here, it is not
tracked.**

Next free ID: **Q-015**. IDs are never reused.

## Open

| ID | Topic path | Question | Blocks | Decide by | Detail |
|---|---|---|---|---|---|
| Q-001 | interface / desktop | Qt or GTK for the desktop frontend? | F-017 | M8 — cheap to defer | [detail](interface/desktop/Q-001-toolkit-choice.md) |
| Q-002 | build-and-release / testing | Catch2 or doctest for tests? | test setup | M0 — needed now | — |
| Q-003 | mathematics / precision | Own arbitrary-precision arithmetic, or GMP/MPFR? | F-010 | M5 — needs research on licensing | — |
| Q-004 | foundation / value-model | How does `Value` represent a united quantity? | F-012, F-019 | before M5 — expensive after | [detail](foundation/value-model/Q-004-value-and-units.md) |
| Q-005 | expression-engine / evaluation | Is angle mode global state, or part of a call? | F-005, F-013 | M2 | — |
| Q-006 | extensibility / plugin-system / abi | Must ABI v1 be able to pass arrays? | F-015, F-016 | **before ABI v1 ships — irreversible after** | [detail](extensibility/plugin-system/abi/Q-006-abi-arrays.md) |
| Q-007 | interface / session-state | Workspace file format — JSON, TOML, or custom? | F-018 | M8 | — |
| Q-008 | expression-engine / evaluation | Walk the tree every time, or compile it once? | F-002, F-013 | before M6 — but shapes the AST in M1 | [detail](expression-engine/evaluation/Q-008-evaluation-strategy.md) |
| Q-009 | expression-engine / evaluation | How does a user interrupt a long computation? | F-002, F-013, F-017 | **M1 — it is part of the evaluator's signature** | [detail](expression-engine/evaluation/Q-009-cancellation.md) |
| Q-010 | foundation / error-handling | Does `Diagnostic` carry English text, or a message id? | F-006 | M3 — before it is widely used | [detail](foundation/error-handling/Q-010-diagnostic-and-translation.md) |
| Q-011 | expression-engine / notation-handling | Does the parser stop at the first error, or collect several? | F-002, F-006 | M3 — changes `Diagnostic` from one to many | — |
| Q-012 | foundation / value-model | Is the engine single-threaded? Must the registry be thread-safe? | F-013, F-015, F-017 | M6 — plot sampling is the first parallel workload | — |
| Q-013 | extensibility / plugin-system | Are operators registry entries, so a plugin can add one? | F-003, F-015 | before ABI v1 — needs a dynamic precedence table | — |
| Q-014 | foundation / value-model | Who owns number formatting — significant digits, when to go scientific? | F-007, F-017 | M4 — both frontends need identical output | — |

## Answered

*None yet.* Answered questions move here with their resolution and a link to
whatever settled them.

Format:

> **Q-000 · Should modules nest?** — Not in `libs/`.
> [ADR 0001](../adr/project-structure/module-layout/0001-modules-with-strict-layering.md):
> a module's folder path is one of four places its name appears, so nesting
> breaks that mapping. Documentation folders are different and nest freely —
> [ADR 0008](../adr/project-structure/knowledge-base/0008-knowledge-base-structure.md)
> explains why the two rules do not conflict.

---

## Reading the "Decide by" column

Not all deferral is equal, and the difference matters more than urgency:

- **Cheap to defer** — Q-001. ADR 0005 keeps `libs/plot` toolkit-free, so the
  choice stays reversible. Deferring costs nothing.
- **Needed soon** — Q-002, Q-005. Work is blocked, but a wrong answer is
  correctable.
- **Irreversible at a specific moment** — Q-006, Q-013. Once ABI v1 is published
  the contract is frozen forever. These are the ones to watch.

### The signature questions

Q-009 and Q-010 are a distinct kind, and the most likely to be missed. Neither
needs its *mechanism* built early — but both change a **type signature** used in
hundreds of places, so the shape has to be right in M1 and M3 respectively.
Deciding them late is not a feature delay, it is a sweep through every call site
in three modules.
