# ccalc — command-line application

The terminal calculator. This is the first frontend and the one that gets built
first.

## Two modes

**REPL** — run `ccalc` with no arguments. Read a line, evaluate, print, repeat,
until `:quit` or end of input.

```
> 2 + 3 * 4
14
> x = 5
5
> sqrt(x^2 + 12^2)
13
```

**One-shot** — `ccalc "2 + 3 * 4"` evaluates, prints, exits. This makes CCalc
usable in shell scripts, and it makes end-to-end testing trivial: run the binary,
check stdout and the exit code.

## What goes here

- `main.cpp` — argument parsing, choosing the mode
- The REPL loop, prompt, and line editing (history, arrow keys, completion)
- Formatting results and errors for a terminal, including the caret line:
  ```
  > 2 + * 4
        ^ expected a number, found '*'
  ```
- Colour output, and detecting when output is a pipe so colour is disabled
- Terminal plot rendering — taking `PlotData` from `libs/plot` and drawing it
  with Unicode block characters
- REPL commands: `:help`, `:vars`, `:plugins`, `:quit`

## What does NOT go here

- Anything from the list above in `apps/README.md`. No parsing, no evaluation,
  no plot geometry.
- The tick-choosing and sampling maths behind a terminal plot. This app receives
  finished geometry and turns it into characters — nothing more.

## Exit codes

| Code | Meaning |
|---|---|
| 0 | Success |
| 1 | Evaluation error (bad expression, unknown name, division by zero) |
| 2 | Usage error (bad arguments) |

Scripts depend on these, so treat them as part of the interface.
