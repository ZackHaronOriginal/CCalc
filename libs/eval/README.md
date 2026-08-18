# eval

**Layer L2** · target `ccalc_eval` · namespace `ccalc::eval` · `<ccalc/eval/…>`

Walks a syntax tree and produces a value. The heart of the calculator.

## What goes here

- `Evaluator` — recursive post-order walk of the tree: evaluate the children,
  apply the operator
- `Environment` — the variable table, and scope handling if we add user-defined
  functions later
- Assignment (`x = 5`), and the pre-seeded constants `pi`, `e`, `ans`
- Turning a `Diagnostic` from any layer below into something the caller can show

## What does NOT go here

- **The built-in functions themselves.** `sqrt` lives in `functions`. This module
  resolves the *name* `sqrt` through the registry and calls whatever comes back.
- Mathematics. `numeric` owns the algorithms.
- Parsing. `expr` owns that.
- **Anything about plotting.** See below — this is the rule most likely to get
  broken.

## The rule most likely to be broken

Sooner or later you will add `graph(sin(x))` as something the user can type, and
the obvious implementation is for `eval` to call `plot`. **Do not.** `plot` is
L3, this module is L2, and that call points upward.

Instead, evaluating `graph(...)` produces a *result value* meaning "this is a
plot request for this expression". The app layer sees that result and calls
`plot` itself. The feature works identically, and both modules still build alone.

## How plugin functions work without this module knowing

`eval` asks the registry (owned by `functions`) for a name and calls what comes
back. It has no idea whether that came from built-in code compiled a month ago
or from a `.so` file loaded three seconds ago. `plugin-host` fills the registry
at startup, from a layer above.

That indirection is the entire reason the plugin system can exist without
anything in L0–L2 changing. Do not add a direct call from here to
`plugin-host` — it would defeat the design and create a cycle.
