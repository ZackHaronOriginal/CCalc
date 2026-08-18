# .github/

GitHub-specific configuration. Nothing in here is part of the program.

## What goes here

- `workflows/` — CI pipeline definitions (see its own README)
- `ISSUE_TEMPLATE/` — bug report and feature request templates, when we add them
- `pull_request_template.md` — the checklist contributors fill in
- `CODEOWNERS` — who reviews what, once there is more than one person

## What does NOT go here

- Project documentation. That belongs in `docs/`.
- Build or release scripts. Those go in `tools/`, and the workflow *calls* them.

## Why the split matters

A CI workflow should be a thin file that calls a script in `tools/`. If the logic
lives inside the YAML, you can only test it by pushing a commit and waiting.
If it lives in `tools/`, you can run it on your own machine in two seconds.
