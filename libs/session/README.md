# session

**Layer L3** · target `ccalc_session` · namespace `ccalc::session` · `<ccalc/session/…>`

The user's ongoing state: what they have typed, what they have defined, and what
persists after they close CCalc.

## What goes here

- `History` — the list of past entries and results, and searching through it
- `ans` — the previous result, and the rules for what updates it
- Workspace save and load: variables and definitions written to disk and
  restored on the next launch
- Session-scoped preferences — output precision, angle mode (degrees or radians)
- Import and export of a session as a readable file

## What does NOT go here

- The variable table itself. That is `Environment`, and it lives in `eval`. This
  module *persists* an environment; it does not implement one.
- Terminal line editing, arrow-key handling, tab completion. Those are frontend
  concerns and belong in `apps/cli`. This module owns the history *data*; the
  app owns how a person navigates it.

## Why it is a module rather than part of an app

Both the CLI and the GUI need history and saved workspaces, and they must agree
about the file format — a workspace saved in the GUI has to open in the CLI. Put
it in one module and that is guaranteed. Put it in each app and it becomes two
implementations that drift apart.

## Rule for the file format

Whatever format we pick, write a version number as the first field, and make the
loader refuse a file it does not understand rather than guessing. Users will have
saved workspaces they care about; silently misreading one is much worse than
declining to open it.
