# F-015. Plugin ABI and host

**Topic:** `plugins` · **Status:** planned
**Milestone:** M7 · **Related:** ADR 0006, R-001, R-003, F-003, F-016

## What

Someone writes a `.so`, drops it in a plugin directory, restarts CCalc, and their
function works — with no recompilation of CCalc and no approval from us.

## Why

Extensibility is a stated goal of the project. It also keeps heavy or niche
features optional: a symbolic-algebra plugin can pull in a large dependency that
users who do not want it never install.

## Scope

**In:** `libs/plugin-abi/` (the C contract, header-only, L0);
`libs/plugin-host/` (discovery, `dlopen`, version checking, registration);
the directory search order; the plugin author guide in `docs/PLUGINS.md`.

**Out:** sandboxing — impossible for native code without a much larger project;
scripting plugins, which are a separate future feature; a plugin repository or
installer.

## Design notes

Settled by ADR 0006 and R-001:

- Plain C at the boundary. No C++ type crosses it.
- `abi_version` is the first struct field.
- Whoever allocates, frees.
- No exception escapes a plugin.
- Search order from R-003; never the working directory.

The system splits into **two modules at opposite ends of the stack** — the
contract at L0 depending on nothing, the host at L3 depending on nearly
everything. That is what stops a plugin binding to the whole engine.

## The sequencing that limits the risk

The expensive, irreversible step is not writing the loader — it is **publishing
the contract.** Once an external plugin ships, `plugin-abi` can never change
incompatibly again.

1. Registry first (F-003, M2) — everything attaches to it
2. Sketch `plugin-abi` early, and keep it honest by routing our own built-ins
   through it
3. Add `plugin-host` once value and function types have stopped moving
4. Write our own plugins first (F-016) to find the awkward parts
5. **Declare ABI version 1 only when ready to keep that promise permanently**

## Open questions

- Q-006: can arrays be passed in v1? F-016 is the test case, and this must be
  settled *before* publication, not after.
- Should plugins be loadable at runtime without a restart? Simpler to require a
  restart; unloading a library whose data is still referenced crashes at exit.

## Done when

- A plugin built **with a different compiler than the host** loads and works
- A plugin with a wrong `abi_version` is refused with a clear message, and CCalc
  keeps running normally
- A plugin that fails to load never takes CCalc down
- `plugins/example/` builds against the *installed* SDK, with no reference to
  this source tree
- `docs/PLUGINS.md` states plainly that plugins are unsandboxed native code
