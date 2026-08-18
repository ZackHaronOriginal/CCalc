# project

**Layer:** — · **Directories:** repository-wide

Structure, conventions and process — anything that spans modules rather than
living in one.

## In scope

The `libs/` layout and the layer rule. Header placement. Naming conventions. The
knowledge base itself: how ADRs, research, features, bugs and questions work.
Commit and documentation practice. The topic vocabulary.

## Not in scope

Anything belonging to a single module. If it changes one module and nothing else,
it has a more specific topic.

## The guard against misuse

`project` is for genuinely cross-cutting concerns — **not a dumping ground for
anything hard to classify.** If you reach for it twice in a row, the topic list
probably needs a real addition instead.

The honest test: *would this still be true if we deleted half the modules?*
Structure and process survive that; a decision about matrices does not.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `build` | Source organisation → `project`. Compiling and shipping → `build` |
| `core` | `core` is a module with code; `project` has no code at all |

## Decisions in force

ADR 0001 — layered modules under `libs/`.
ADR 0004 — separate `include/` and `src/` per module.

## Research

R-002 — the real standards for C++ project layout.
