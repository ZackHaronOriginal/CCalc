# 2. Use a Pratt parser rather than shunting-yard

**Status:** Accepted
**Date:** 2026-08-18

## Context

CCalc must parse infix expressions with several precedence levels, unary minus,
right-associative exponentiation, function calls with arguments, and assignment.
The classic textbook answer for a calculator is Dijkstra's shunting-yard
algorithm.

## Decision

Use a Pratt parser (precedence climbing) in `libs/expr/`, producing a real
syntax tree.

## Alternatives considered

**Shunting-yard.** The famous choice, and genuinely elegant for pure binary
infix arithmetic. Rejected because it degrades quickly once the grammar grows:
unary minus needs a special case, right-associative `^` needs another, and
function calls with a variable number of arguments need a third. It also produces
a flat output stream rather than a tree, which is awkward for a graphing
calculator that needs to hold and re-evaluate an expression many times.

**A parser generator** (Bison, ANTLR). Rejected as disproportionate: it adds a
build dependency and a generated-code step for a grammar that is roughly 120
hand-written lines, and it makes good error messages harder rather than easier.

**Plain recursive descent with one function per precedence level.** Perfectly
workable. Rejected only because adding a precedence level means adding a
function and rewiring its neighbours, where Pratt makes it one table entry.

## Consequences

Adding an operator is a one-line change to a precedence table. Unary minus,
right associativity and calls fall out naturally. We get a real AST, which
`plot` needs in order to re-evaluate `f(x)` thousands of times cheaply.

The cost is that precedence climbing is less widely recognised than
shunting-yard, so the code needs comments explaining the binding-power idea for
anyone meeting it for the first time.

`docs/GRAMMAR.md` is the written form of what the parser accepts, and must change
in the same commit as the parser.
