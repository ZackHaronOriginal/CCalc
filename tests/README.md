# tests/

**Integration and end-to-end tests only.** Tests that cross module boundaries.

Unit tests do **not** live here — they live inside the module they test, at
`libs/<name>/tests/`.

## The difference, concretely

| Kind | Lives in | Asks |
|---|---|---|
| Unit | `libs/expr/tests/` | Does the parser read `2+3*4` with correct precedence? |
| Integration | `tests/` | Does typing `2+3*4` into the CLI print `14`? |

A unit test knows about one module and fails with a precise cause. An
integration test knows about the whole program and tells you the pieces are
wired together correctly.

## What goes here

- `cli/` — drive `ccalc` in one-shot mode, check stdout and the exit code
- `pipeline/` — expression text in, evaluated result out, across expr+eval+functions
- `plugins/` — build a test plugin, load it, confirm its function is callable
- `fixtures/` — shared input files and expected outputs

## What does NOT go here

- Anything testing one module in isolation.
- Performance benchmarks. Those go in `tools/bench/`.

## Rules

- Every integration test must be runnable by `ctest` with no manual setup.
- Test error cases as seriously as success cases: unbalanced parens, unknown
  names, division by zero, a plugin with a wrong ABI version.
- When an integration test fails, add the unit test that would have caught it
  earlier. Integration tests tell you *that* something broke; unit tests tell
  you *where*.
