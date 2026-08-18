# Tokenizer plan

**Topic:** expression-engine / lexical-handling · **Status:** draft
**Related:** F-001, ADR 0002, `libs/expr/`

How CCalc turns raw input text into a token stream.

## Shape

A single forward pass over the input, one character of lookahead, no regex and no
backtracking. The lexer never fails outright — an unrecognisable character
produces an error token carrying its position, so the parser can report it in
context rather than the lexer aborting the line.

## Token kinds

| Group | Kinds |
|---|---|
| Literals | `Number` |
| Names | `Identifier` |
| Operators | `Plus` `Minus` `Star` `Slash` `Percent` `Caret` |
| Grouping | `LParen` `RParen` `Comma` |
| Assignment | `Equals` |
| Units | `Arrow` (`->`), for `5 km -> miles` |
| Control | `End`, `Error` |

## Every token carries its position

A token stores the byte offset and length of the text it came from. This is not
optional detail — it is what makes this possible:

```
> 2 + * 4
      ^ expected a number, found '*'
```

Retrofitting positions later means touching every token construction site, so
they go in from the first line.

## Open points

- **Number literal forms** — see `number-literal-forms.md`. Hex, binary and
  scientific notation each affect the lexer loop.
- **Whitespace and implicit multiplication.** Does `2x` mean `2 * x`? Convenient,
  and standard in written mathematics — but it makes `2 km` ambiguous once units
  exist. Probably a question for `questions/`, not a decision to take here.
- **Unicode identifiers.** Allowing `π` and `α` is friendly; it also means the
  character loop stops being byte-oriented. See `unicode-identifiers.md`.

## Testing approach

String in, expected token sequence out. Cheap to write and exhaustive, so this
should be the most thoroughly tested part of the project. Include the awkward
inputs: empty string, whitespace only, a lone operator, an unterminated number
like `1.2.3`, and a character outside the alphabet entirely.
