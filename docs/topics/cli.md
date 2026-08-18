# cli

**Layer:** app · **Directory:** [`apps/cli/`](../../apps/cli/README.md)

The terminal frontend: REPL, one-shot mode, and terminal rendering.

## In scope

Argument parsing and mode selection. The REPL loop, prompt, line editing,
history navigation, completion. Formatting results and errors for a terminal,
including the caret line under an error column. Colour, and disabling it when
output is piped. **Drawing** plots with Unicode block characters. Commands like
`:help`, `:vars`, `:plugins`, `:quit`. Exit codes.

## Not in scope

Anything worth testing on its own. No parsing, no evaluation, no plot geometry.
If both frontends would want it, it belongs in `libs/`.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `plot` | Where a tick goes → `plot`. Which character draws it → `cli` |
| `session` | The history data → `session`. Arrow-key navigation → `cli` |
| `eval` | Computing the answer → `eval`. Formatting it to 6 significant figures → `cli` |

## Note on exit codes

`0` success, `1` evaluation error, `2` usage error. Scripts depend on these, so
treat them as interface, not implementation.
