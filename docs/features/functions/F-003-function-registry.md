# F-003. Function registry

**Topic:** `functions` · **Status:** planned
**Milestone:** M2 · **Related:** ADR 0006, F-015, R-001

## What

A table mapping a name the user types — `sqrt`, `stddev` — to something callable.
`eval` asks the table for a name and calls whatever comes back.

From the user's side this is invisible. It is the mechanism that makes `sqrt(2)`
work at all, and later makes a plugin's function work identically.

## Why

Two reasons, and the second is the important one.

**Immediately:** it is how any named function gets called. Without it, `eval`
would need a hardcoded `if` chain over every function name.

**Structurally:** it is the seam the entire plugin system attaches to. Because
`eval` (L2) only ever talks to this table, it never learns that plugins exist.
`plugin-host` (L3) fills the table at startup. Nothing in L0–L2 changes when the
plugin system is added — it is purely additive.

That is why this lands in M2 rather than M7 with the rest of the plugin work. It
costs a few hours, improves the core regardless, and everything else hangs on it.

## Scope

**In:** the name-to-callable table; lookup; registration; `Signature` (arity and
argument kinds) so wrong calls give a clear error rather than crashing;
registering all built-ins at startup.

**Out:** the functions themselves (F-005); the mathematics behind them, which
lives in `numeric`; loading anything from disk (F-015).

## Design notes

- Lives in `libs/functions/`, layer L2.
- **Register CCalc's own built-ins through the same interface a plugin would
  use** — not a parallel internal path. If our own `sqrt` cannot be expressed
  through the plugin interface, no plugin will manage it either. Doing it this
  way means every build exercises the plugin path, so it cannot quietly rot.
- Arity and argument-kind checking belongs here, so every caller gets consistent
  errors without duplicating validation.
- Lookup is on the hot path for graphing — plotting a curve calls the same
  function thousands of times — so the table must be cheap to read. Design for
  many reads and rare writes.

## Open questions

- Q-006: does the registry signature need to carry arrays in v1? Statistics
  functions take a list, and that shapes the plugin ABI. Deciding late is
  expensive because the ABI freezes on publication.

## Done when

- `sqrt(2)` resolves through the registry and returns 1.41421…
- Calling a function with the wrong number of arguments produces a clear
  diagnostic pointing at the call, not a crash
- An unknown name produces "unknown function 'foo'" with the column marked
- Every built-in is registered through the same path a plugin would use
- `eval` contains no reference to any specific function name
