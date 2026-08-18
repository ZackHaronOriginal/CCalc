# .github/workflows/

CI pipeline definitions. Each `.yml` file here is one workflow that GitHub runs
automatically.

## What goes here

- `ci.yml` — the main one: configure, build, run `ctest` on every push and PR
- `release.yml` — later: build packages and attach them to a tagged release
- `lint.yml` — later: clang-format and clang-tidy checks

## Planned CI matrix

| Axis | Values |
|---|---|
| OS | ubuntu-latest (primary), later macos-latest |
| Compiler | GCC 14, Clang 18 |
| Build type | Debug (with warnings-as-errors), Release |

Two compilers is not redundancy. GCC and Clang disagree about which C++23
features are ready and which warnings matter, and building on both catches real
portability bugs early — which matters more than usual for us, because plugin
authors will not all use your compiler.

## Rules

- Keep the YAML thin. Anything longer than a few lines becomes a script in `tools/`.
- CI must fail on warnings. A warning nobody fixes is a warning everybody ignores.
- Never mark a test as skipped to make CI green. Fix it or revert the change.
