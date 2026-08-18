# Research index

Every research entry. **If it is not listed here, it does not exist.**

Next free ID: **R-004**. IDs are global and never reused.

| ID | Topic path | Question | Status | Findings |
|---|---|---|---|---|
| R-003 | extensibility / plugin-system / discovery | Where do plugins belong on disk on Linux? | complete | [findings](extensibility/plugin-system/discovery/R-003-linux-plugin-discovery/README.md) |
| R-002 | project-structure / layout-standards | What are the real standards for C++ project layout? | complete | [findings](project-structure/layout-standards/R-002-cpp-project-layout-standards/README.md) |
| R-001 | extensibility / plugin-system / abi | Why does C++ have no stable ABI, and what does that force? | complete | [findings](extensibility/plugin-system/abi/R-001-cpp-abi-stability/README.md) |

## Status values

| Status | Meaning |
|---|---|
| `active` | in progress; conclusions not yet reliable |
| `complete` | concluded; the answer is in the README |
| `superseded by R-NNN` | later work replaced it; kept for the record |

## Suggested next research

Not started. Each blocks a question in [`../questions/INDEX.md`](../questions/INDEX.md).

| Suggested topic path | Question | Blocks |
|---|---|---|
| mathematics / precision | Own arbitrary-precision code, or GMP/MPFR? Licensing, size, packaging | Q-003 |
| interface / desktop | Qt vs GTK for a graphing frontend: licensing, packaging, canvas quality | Q-001 |
| graphing / geometry / tick-selection | Wilkinson's extended algorithm vs simpler nice-numbers | F-013 |
| graphing / geometry / sampling | Adaptive sampling and asymptote detection for pathological functions | F-013 |
| build-and-release / testing | Catch2 vs doctest: compile time, output quality, CTest integration | Q-002 |
| interface / session-state | Workspace file format — JSON, TOML, or a custom line format | Q-007 |
