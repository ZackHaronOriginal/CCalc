# 3. Report errors with std::expected, not exceptions

**Status:** Accepted
**Date:** 2026-08-18

## Context

Every layer of a calculator can fail: the lexer meets a stray character, the
parser meets an unbalanced paren, the evaluator meets an unknown name or a
division by zero. These failures must reach the user with a message and a
position precise enough to print a caret under the offending character.

We target C++23, so `std::expected` is available.

## Decision

Fallible operations return `std::expected<T, Diagnostic>`, aliased as
`Result<T>` in `core`. Exceptions are reserved for genuine programming errors
and allocation failure.

## Alternatives considered

**Exceptions.** The conventional C++ answer. Rejected because in a calculator,
malformed input is not exceptional — it is the single most common thing that
happens. Users typo constantly. Wrapping every REPL line in `try`/`catch` puts
the error path out of sight, and the type signatures stop telling you what can
fail.

**Error codes with out-parameters.** Rejected: forgettable at call sites, and it
makes composing operations noisy.

**A custom `Result` type.** Rejected — that is `std::expected`, and there is no
reason to hand-roll it now that it is standard.

## Consequences

"This can fail" is visible in every signature. The error path is ordinary code,
so it gets the same attention as the success path. No unwinding cost per keypress.

It also matters for plugins: an exception that escapes a plugin across the C ABI
boundary is undefined behaviour. A codebase already in the habit of returning
errors as values adapts to that constraint naturally.

The costs: more explicit propagation, and `Diagnostic` must be designed early
because it appears in a great many signatures. Changing it later is a wide edit.
