# eval

**Layer:** L2 · **Module:** [`libs/eval/`](../../libs/eval/README.md)

Walking the syntax tree and producing a value. Variables, assignment, scope, the
environment.

## In scope

**Meaning.** What an expression computes, what a name refers to, what happens on
division by zero, how a `Diagnostic` from a lower layer reaches the caller.

## Not in scope

The functions themselves (`functions`), the mathematics (`numeric`), the grammar
(`expr`), and — importantly — **anything about plotting**.

## The rule most likely to be broken

Adding `graph(sin(x))` as a typed command tempts you to have `eval` call `plot`.
`plot` is L3 and this is L2, so that call points upward and creates a cycle.

Evaluating `graph(...)` must produce a *result value* meaning "this is a plot
request". The app acts on it. Same feature; no cycle.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `expr` | Syntax → `expr`. Semantics → `eval` |
| `functions` | The lookup table → `functions`. Deciding *to* look something up → `eval` |
| `session` | The live variable table → `eval`. Persisting it to disk → `session` |

## Open

Q-005 — is angle mode global state or part of a call?
