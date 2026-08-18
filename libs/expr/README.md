# expr

**Layer L1** · target `ccalc_expr` · namespace `ccalc::expr` · `<ccalc/expr/…>`

Turns text into a syntax tree. **Understands grammar, not meaning.**

## What goes here

- `Token` and the token kinds
- `Lexer` — a character loop that turns `"2 + 3"` into `NUM(2) PLUS NUM(3) EOF`.
  Every token carries its column so errors can point at the right character.
- `Ast` — the node types: `Number`, `Variable`, `Unary`, `Binary`, `Call`,
  `Assign`. Six types, deliberately few.
- `Parser` — a **Pratt parser** (precedence climbing), producing the tree.

## Why a Pratt parser and not shunting-yard

Shunting-yard produces a flat output stream and becomes awkward the moment you
add unary minus, right-associative `^`, or function calls. A Pratt parser handles
all three naturally, produces a real tree, and is about the same size —
roughly 120 lines, with each operator's precedence as one table entry. Adding an
operator later is a one-line change.

## What does NOT go here

- **Any evaluation.** This module never computes `2 + 3`. It produces a tree that
  says "add these two things" and stops. Evaluation is `eval`'s job.
- Any mathematics. `numeric` owns that.
- Any knowledge of which functions exist. `sqrt(2)` parses into a `Call` node
  whether or not `sqrt` is a real function. Deciding that is `eval`'s job.

## Keep the grammar in sync

`docs/GRAMMAR.md` is the written form of what this module accepts. If you change
the parser, change the grammar file in the same commit.

## Testing note

The best parser test compares against a canonical parenthesised rendering of the
tree: `"2+3*4"` should produce `"(2 + (3 * 4))"`. One string comparison proves
precedence and associativity, which is exactly where parser bugs hide.
