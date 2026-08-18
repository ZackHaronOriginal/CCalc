# apps/

The executables. Each subdirectory builds one program a user can run.

## The one rule

> **Apps contain no logic.**

An app reads command-line arguments, sets up input and output, calls into
`libs/`, and displays what comes back. That is the entire job.

The test is simple: **if you would ever want to write a test for it, it does not
belong in an app.**

## Why this matters more than it looks

Suppose the CLI parses numbers directly in its input loop, because it is only a
few lines. Now:

- Testing that `2 + 3` gives `5` means launching the whole program, feeding it
  standard input, capturing standard output and comparing strings. That test is
  slow, fragile, and breaks when you change the prompt character.
- When the GUI arrives, none of it can be reused. You write it again, and now
  there are two subtly different parsers that will eventually disagree.

With the logic in `libs/eval`, the test is one line — call `evaluate("2 + 3")`,
check it returns 5 — and both frontends call the same function, so they can
never disagree about what an expression means.

## What an app may contain

- `main()` and argument parsing
- Reading input and writing output
- Terminal handling, or window and widget code
- Turning a `Diagnostic` into a nicely formatted error message
- Wiring: create the environment, load plugins, connect the pieces

## What an app may NOT contain

- Parsing, evaluation, mathematics, unit conversion, plot geometry
- Anything another app would also need. If both the CLI and the GUI want it, it
  belongs in `libs/`.

## Apps may use anything

An app sits above every layer and may depend on any module. The reverse is never
true: **nothing in `libs/` may ever depend on anything in `apps/`.**
