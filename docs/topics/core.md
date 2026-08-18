# core

**Layer:** L0 · **Module:** [`libs/core/`](../../libs/core/README.md)

Shared types every other module depends on: `Value`, `Diagnostic`, `Result<T>`,
configuration, logging.

## In scope

The vocabulary of the project. What a result *is*. What an error *is*. How
failures travel. Anything a module in any layer needs to speak to another.

## Not in scope

**Anything that knows what a calculator is.** No parsing, no evaluation, no
mathematics. The test: *would this still make sense in a program that was not a
calculator?* If no, it is not `core`.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `numeric` | `core` defines what a `Value` *is*; `numeric` does mathematics *to* numbers |
| `project` | `core` is a module; `project` is process and structure spanning modules |

## Watch out

`core` is where tired people put anything that does not obviously fit. Left
unchecked it becomes a junk drawer that everything depends on, so every change
rebuilds the world. Be suspicious of additions here.
