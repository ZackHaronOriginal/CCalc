# Research index

Every research entry. **If it is not listed here, it does not exist.**

Next free ID: **R-004**. IDs are global and never reused.

| ID | Topic | Question | Status | Findings |
|---|---|---|---|---|
| R-003 | plugins | Where do plugins belong on disk on Linux? | complete | [findings](plugins/R-003-linux-plugin-discovery/README.md) |
| R-002 | project | What are the real standards for C++ project layout? | complete | [findings](project/R-002-cpp-project-layout-standards/README.md) |
| R-001 | plugins | Why does C++ have no stable ABI, and what does that force? | complete | [findings](plugins/R-001-cpp-abi-stability/README.md) |

## Status values

| Status | Meaning |
|---|---|
| `active` | in progress; conclusions not yet reliable |
| `complete` | concluded; the answer is in the README |
| `superseded by R-NNN` | later work replaced it; kept for the record |

## Suggested next research

Not started. Each of these blocks a question in [`../questions/INDEX.md`](../questions/INDEX.md).

| Topic | Question | Blocks |
|---|---|---|
| `numeric` | Own arbitrary-precision code, or GMP/MPFR? Licensing, size, packaging | Q-003 |
| `gui` | Qt vs GTK for a graphing frontend: licensing, packaging, canvas quality | Q-001 |
| `plot` | Tick-interval algorithms — Wilkinson's extended algorithm vs simpler nice-numbers | F-010 |
| `plot` | Adaptive sampling and asymptote detection for pathological functions | F-010 |
| `build` | Catch2 vs doctest: compile time, output quality, CTest integration | Q-002 |
| `session` | Workspace file format — JSON, TOML, or a custom line format | Q-007 |
