# core

**Layer L0** · target `ccalc_core` · namespace `ccalc::core` · `<ccalc/core/…>`

The foundation everything else stands on. **Depends on nothing.**

## What goes here

- `Value` — the type a calculation produces. Start as a wrapper around `double`;
  it becomes a `std::variant` later when integers, complex numbers and matrices
  arrive. Wrapping it now costs nothing and saves editing every call site later.
- `Diagnostic` — an error: a message, a column position, a length. This is what
  lets the REPL print a caret under the exact character that went wrong.
- `Result<T>` — the alias for `std::expected<T, Diagnostic>` used everywhere.
- Configuration loading and the settings type.
- Logging.
- Small shared utilities with no better home — string helpers, source spans.

## What does NOT go here

- **Anything that knows what a calculator is.** No parsing, no evaluation, no
  mathematics. If it mentions expressions or numbers-as-maths, it belongs in a
  higher layer.
- Anything that would make `core` depend on another module. It depends on
  nothing, and that must stay true.

## The danger with this module

`core` is where tired developers put things that do not obviously fit anywhere
else. Left unchecked it becomes a junk drawer that every other module depends on,
which means every change to it rebuilds the entire project.

Before adding something here, ask: **would this still make sense in a program
that was not a calculator?** If no, it goes somewhere else.

## Note on errors

We use `std::expected<T, Diagnostic>`, not exceptions. In a calculator, bad input
is the *normal* case — users typo constantly — and exceptions are for the
exceptional. `std::expected` also makes "this can fail" visible in every
signature, which matters when a value crosses into plugin code that must never
let an exception escape.
