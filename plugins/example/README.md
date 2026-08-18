# example — reference plugin

The smallest possible working plugin. Its job is to be **read and copied**, not
to be useful.

This is what we point third-party authors at, so it is documentation as much as
code. Optimise it for being understood in five minutes.

## What it does

Adds one trivial function — something like `double(x)` returning `x * 2`. The
function is deliberately boring so nothing distracts from the mechanics.

## What it demonstrates

1. Including `<ccalc/plugin/abi.h>` and nothing else from CCalc
2. Filling in `CCalcPluginInfo` with `abi_version` first
3. Exporting `ccalc_plugin_entry` as `extern "C"`
4. Registering a function with the registry
5. Wrapping the implementation in `try`/`catch` so no exception escapes
6. A `CMakeLists.txt` that builds a shared library against the installed SDK —
   with no reference to the CCalc source tree

Point 6 matters more than it looks. This build file must work for someone who
installed `ccalc-dev` from their package manager and has never seen our
repository. If it only builds inside this tree, it is not a usable example.

## Rules

- Keep it tiny. Every feature added here is something a newcomer must read past.
- Comment generously — far more than normal project style. The comments are the
  point.
- It must always build. If an ABI change breaks it, that change is not finished.
- When the ABI changes, this is the first thing updated.
