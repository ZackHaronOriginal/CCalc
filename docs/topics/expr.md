# expr

**Layer:** L1 · **Module:** [`libs/expr/`](../../libs/expr/README.md)

Turning text into a syntax tree: tokens, the lexer, the AST node types, the Pratt
parser, and the grammar.

## In scope

Everything about **form**. What is syntactically valid, what precedence an
operator has, what error a malformed input produces, what the tree looks like.
`docs/GRAMMAR.md` is this topic's specification.

## Not in scope

**Meaning.** `expr` never computes `2 + 3`; it produces a tree saying "add these"
and stops. It also does not know which functions exist — `sqrt(2)` parses into a
call node whether or not `sqrt` is real.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `eval` | If it is about *what the text is allowed to look like* → `expr`. If it is about *what the text means* → `eval` |
| `functions` | Parsing a call is `expr`; resolving the name is `functions` |

**Worked example:** "`sqrt(-1)` returns NaN instead of erroring" is `eval` or
`numeric`, not `expr` — the text parsed fine. "`2 +` is accepted with a trailing
operator" is `expr`.

## Decisions in force

ADR 0002 — Pratt parser rather than shunting-yard.
