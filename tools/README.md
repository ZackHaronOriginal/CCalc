# tools/

Scripts and small programs used during *development*. Never shipped to users.

## What goes here

- `bench/` — performance benchmarks (parser throughput, plot sampling speed)
- `format.sh` — run clang-format across the tree
- `lint.sh` — run clang-tidy with the project's checks
- `new-module.sh` — scaffold a new `libs/` module with the standard shape
- `gen-*.py` — code generators, if we ever need generated tables
- CI helper scripts that `.github/workflows/` calls

## What does NOT go here

- Anything the installed program needs. That is `data/`.
- Anything that is part of the build. That is `cmake/`.

## Why CI scripts live here and not in the workflow YAML

A script in this folder can be run on your own machine in two seconds. The same
logic written inline in a GitHub workflow can only be tested by pushing a commit
and waiting for the runner. Keep the YAML thin and the logic here.
