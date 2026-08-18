# Number literal forms

**Topic:** expression-engine / lexical-handling · **Status:** draft
**Related:** F-001, `tokenizer-plan.md`

Which numeric spellings the lexer accepts.

## Proposed for M1

| Form | Example | Notes |
|---|---|---|
| Integer | `42` | |
| Decimal | `3.14` | |
| Leading dot | `.5` | Accept — common and unambiguous |
| Trailing dot | `5.` | Accept, but see the conflict below |
| Scientific | `1.5e-3` | `e` immediately after a digit or dot |

## Later

| Form | Example | Blocked on |
|---|---|---|
| Hexadecimal | `0xFF` | nothing — cheap to add |
| Binary | `0b1010` | nothing |
| Digit separators | `1_000_000` | choice of separator character |
| Arbitrary precision | `1.23456789…` | F-010 |

## The conflicts worth knowing about

**`5.` versus a future range operator.** If `..` ever means a range, `5..10`
becomes ambiguous — is that `5.` then `.10`? Resolvable with two characters of
lookahead, but it is easier to decide now than to discover later.

**`1e5` versus an identifier named `e`.** `e` is Euler's number. `1e5` must lex
as one number, not `1 * e * 5`. The rule: `e` is part of the literal only when it
directly follows a digit or a dot *and* is followed by a digit or a sign. So `1e5`
is a number, `1 e5` is two tokens, and `2*e` is multiplication.

**Implicit multiplication makes both worse.** If `2x` means `2 * x`, then `2e`
has to mean `2 * e` while `2e5` stays one number. Workable, but it is a real cost
to weigh when that question is decided.
