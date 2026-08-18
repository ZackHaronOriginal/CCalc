# functions

**Layer L2** · target `ccalc_functions` · namespace `ccalc::functions` · `<ccalc/functions/…>`

The built-in function library **and the registry** — the table that maps a name
the user types to something callable.

## Why the registry lives here

This is the seam the whole plugin system hangs on, so it is worth understanding.

The registry is a plain table: name → callable. `eval` (L2) asks it for
`"stddev"` and calls whatever it gets. `plugin-host` (L3) fills the table at
startup with anything it loaded from disk.

Because `eval` only ever talks to the table, it never learns that plugins exist.
Nothing in L0, L1 or L2 changes at all when the plugin system is added — it is
purely additive. That is the layer rule paying you back.

## What goes here

- `Registry` — the name-to-callable table, and lookup
- `Signature` — how many arguments a function takes and of what kind, so wrong
  calls produce a clear error rather than a crash
- The built-in functions, grouped: `trig`, `log_exp`, `rounding`, `stats`,
  `bitwise`, `random`
- Registration of all built-ins at startup

## What does NOT go here

- The mathematics itself. `numeric` owns the algorithms; this module wraps them
  in something callable by name, checks argument counts, and converts errors.
- Loading plugins. That is `plugin-host`, one layer up.

## The rule that keeps the plugin ABI honest

**Register CCalc's own built-ins through the same interface a plugin would use.**
Not a parallel internal path — the same one.

If your own `sqrt` cannot be expressed through the plugin interface, no plugin
will manage it either. Doing it this way means every build exercises the plugin
path, so it is impossible for it to quietly rot, and you discover awkward parts
of the ABI while it is still free to change.
