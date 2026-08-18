# CCalc

A terminal calculator for Linux — scientific functions, graphing, unit
conversion, and a plugin system for extending it without recompiling.

Early development. The structure is in place; the calculator is not written yet.

## Layout

```
libs/        the calculator itself — one folder per module
apps/        executables (CLI, GUI) — wiring only, no logic
plugins/     first-party plugins, built the same way third-party ones are
tests/       cross-module tests (unit tests live inside each module)
docs/        architecture, grammar, plugin guide
data/        icons, desktop entry, translations
tools/       development scripts and benchmarks
cmake/       shared build helpers
external/    vendored third-party code (prefer FetchContent instead)
packaging/   deb, rpm, flatpak, AppImage
```

**Every folder has a README explaining what belongs in it and what does not.**
Start with [`libs/README.md`](libs/README.md) — it holds the rules that matter.

## The three rules

**1. Dependencies point down.** Modules are layered; a module may only use
modules in a layer strictly below it. Never sideways, never up. This is the rule
that keeps the project workable as it grows, and the one most likely to be
broken by a convenient two-line shortcut.

**2. Apps hold no logic.** If it is worth testing, it lives in `libs/`. The CLI
and the GUI are thin frontends over one shared engine.

**3. `plot/` computes geometry, it does not draw.** The graphing module returns
sample points and tick positions; frontends render them. This is what lets the
same graph appear on a Qt canvas and in a terminal.

## Building

```sh
cmake -B build
cmake --build build
./build/CCalc
```

## Documentation

- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — module layers and the reasoning
- `docs/GRAMMAR.md` — the expression grammar *(to be written)*
- `docs/PLUGINS.md` — guide for plugin authors *(to be written)*

## License

MIT. See [LICENSE](LICENSE).
