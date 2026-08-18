# docs/

Written documentation. Prose for humans, not generated API listings.

## What goes here

- `ARCHITECTURE.md` — the module layers, the dependency rule, why the layout is
  shaped this way. **Read this first.**
- `GRAMMAR.md` — the formal expression grammar, kept in sync with `libs/expr/`
- `PLUGINS.md` — the guide for people writing plugins: the ABI, the rules,
  a walkthrough of the example plugin
- `adr/` — Architecture Decision Records. One short file per significant choice,
  explaining what was decided and *why*. Never delete one; if a decision is
  reversed, write a new record that supersedes it.
- `manual/` — the end-user manual, once there is something to document

## What does NOT go here

- Generated API reference (Doxygen output). That is a build artifact.
- Notes about a single module. Those go in that module's own README.

## The rule that keeps docs honest

If you change the grammar, `GRAMMAR.md` changes in the same commit. If you change
the plugin ABI, `PLUGINS.md` changes in the same commit. Documentation that
drifts out of date is worse than no documentation, because people trust it.
