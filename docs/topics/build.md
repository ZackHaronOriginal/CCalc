# build

**Layer:** — · **Directories:** `cmake/`, `.github/`, `packaging/`, `tools/`

Everything about turning source into something that runs on a user's machine.

## In scope

CMake structure, the module helper, warning flags, build options. CI workflows
and the compiler matrix. Test framework choice and CTest wiring. Packaging for
deb, rpm, Arch, Flatpak, AppImage. Install paths. Development scripts and
benchmarks.

## Not in scope

What the code *does*. A failing test is filed under the topic of the code that
broke, not under `build` — unless the failure is in the build itself.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `project` | How it compiles and ships → `build`. How the source is organised → `project` |
| `plugins` | Where plugins are *installed* → `build`. How they are *loaded* → `plugins` |

## Standing rules

- CI fails on warnings. A warning nobody fixes is a warning everybody ignores.
- **Never skip or disable a test to get a green build.** That turns a visible
  problem into an invisible one.
- Keep workflow YAML thin; logic goes in `tools/` where it can be run locally.

## Open

Q-002 — Catch2 or doctest. Needed at M0.
