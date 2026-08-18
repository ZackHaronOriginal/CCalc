# session

**Layer:** L3 · **Module:** [`libs/session/`](../../libs/session/README.md)

The user's ongoing state: history, `ans`, saved workspaces, session preferences.

## In scope

The history *data* and searching it. What updates `ans`. Writing variables and
definitions to disk and restoring them. Precision and angle-mode preferences.
Import and export of a session.

## Not in scope

**The variable table itself** — that is `Environment`, and it lives in `eval`.
This topic *persists* an environment; it does not implement one. Also not in
scope: arrow keys, tab completion and line editing, which are `cli`.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `eval` | The live table → `eval`. Saving and restoring it → `session` |
| `cli` | The history list → `session`. Navigating it with arrow keys → `cli` |

## Why it is a module, not part of an app

Both frontends need history and workspaces, and a workspace saved in the GUI must
open in the CLI. One module guarantees that; one per app guarantees drift.

## Open

Q-007 — the workspace file format. Whatever it is, version it in the first field
and refuse unknown versions rather than guessing.
