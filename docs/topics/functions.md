# functions

**Layer:** L2 · **Module:** [`libs/functions/`](../../libs/functions/README.md)

The built-in function library **and the registry** — the table mapping a typed
name to something callable.

## Why this topic matters more than its size suggests

The registry is the seam the entire plugin system attaches to. `eval` asks the
table for a name; `plugin-host` fills the table at startup. Because `eval` only
ever talks to the table, it never learns plugins exist — so nothing in L0–L2
changes when plugins arrive.

That is why F-003 lands in **M2**, five milestones before plugins.

## In scope

The registry and lookup. `Signature` — arity and argument kinds — so bad calls
produce clear errors. The built-ins themselves, grouped: trig, log/exp, rounding,
stats, bitwise, random. Registering them at startup.

## Not in scope

The mathematics (`numeric`). Loading anything from disk (`plugins`).

## The rule that keeps the ABI honest

**Register our own built-ins through the same interface a plugin would use** — not
a parallel internal path. If our own `sqrt` cannot be expressed through the plugin
interface, no plugin will manage it either, and we find out while it is still
free to change.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `numeric` | The name and signature → `functions`. The algorithm → `numeric` |
| `plugins` | The table → `functions`. Anything that fills it from a `.so` → `plugins` |
