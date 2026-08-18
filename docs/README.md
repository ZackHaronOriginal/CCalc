# docs/

Two different things live here, with different lifecycles.

## Reference — what we tell people

Curated, kept current, describes how CCalc *is*.

| File | Contents |
|---|---|
| `ARCHITECTURE.md` | module layers, the dependency rule, the reasoning |
| `GRAMMAR.md` | the formal expression grammar *(to be written)* |
| `PLUGINS.md` | the guide for plugin authors *(to be written)* |
| `manual/` | the end-user manual *(to be written)* |

**Rule:** change the thing, change its document, same commit. Documentation that
drifts is worse than none, because people trust it.

## Knowledge base — how we work

Accumulates, is never rewritten, records how CCalc *came to be*.

Six **categories**. Each is one kind of thing, and together they cover everything
worth keeping.

| Category | Holds | Index |
|---|---|---|
| [`research/`](research/) | findings from investigation | [INDEX](research/INDEX.md) |
| [`adr/`](adr/) | decisions, and why the alternatives lost | [INDEX](adr/INDEX.md) |
| [`planning/`](planning/) | plans, interface notes, how things work | [INDEX](planning/INDEX.md) |
| [`features/`](features/) | what we intend to build | [INDEX](features/INDEX.md) |
| [`bugs/`](bugs/) | known defects | [INDEX](bugs/INDEX.md) |
| [`questions/`](questions/) | what we have not decided | [INDEX](questions/INDEX.md) |

## How items are filed: nested topic folders

Inside every category, items live in **topic folders that nest as deep as the
subject needs** — typically two to four levels. Any folder at any depth can hold
as many notes as it takes.

```
docs/planning/                        ← category
└── expression-engine/                ← topic
    └── lexical-handling/             ← subtopic
        ├── tokenizer-plan.md         ← notes, as many as needed
        ├── number-literal-forms.md
        └── unicode-identifiers.md
```

```
docs/adr/                             ← category
└── extensibility/                    ← topic
    └── plugin-system/                ← subtopic
        └── abi/                      ← sub-subtopic
            └── 0006-plugin-boundary-is-c.md
```

**Topic names are free-form and descriptive.** `matrix-handling`,
`notation-handling`, `lexical-handling`, `plugin-system` — name the subject, not
the module. Lowercase with hyphens.

### Any folder may hold notes and subtopics at the same time

Nesting does not push everything down. A topic folder can contain its own files
*and* subtopic folders side by side, at any depth:

```
docs/planning/extensibility/plugin-system/
├── overview.md            ← spans the whole subject
├── versioning-policy.md   ← spans the whole subject
├── abi/                   ← subtopic
│   ├── struct-layout.md
│   └── error-codes.md
├── host/
│   └── loading-sequence.md
└── discovery/
    └── search-order.md
```

**What decides the level:** a note that spans the subtopics stays at the parent;
a note that only concerns one subtopic goes down into it. `versioning-policy.md`
affects the ABI, the host and discovery together, so it belongs at
`plugin-system/`. `struct-layout.md` is only about the ABI, so it belongs in
`abi/`.

Forcing overview notes down into an arbitrary subtopic is the failure mode here.
If a note does not obviously belong in one child folder, it belongs in the
parent.

Create a subtopic when a folder has enough notes that scanning it is annoying —
not before. A folder with two files does not need a subdirectory, and adding one
early just buries them.

### The first level is a fixed list

Below the first level, nest however the subject wants. **The first level is not
free-form** — it is this list, identical in all six categories:

| First-level topic | Covers |
|---|---|
| `expression-engine` | lexing, notation, parsing, evaluation, the grammar |
| `foundation` | value model, error handling, config, logging |
| `mathematics` | numbers, precision, matrices, statistics, units |
| `graphing` | plot geometry, sampling, scales, rendering |
| `extensibility` | the plugin ABI, the host, plugin authoring |
| `interface` | terminal, desktop, session state |
| `project-structure` | layout, conventions, the knowledge base itself |
| `build-and-release` | CMake, CI, testing, packaging |

**Why it is fixed, and not a convention:** because it is what makes one search
find everything about a subject across all six categories at once.

```sh
# every note, decision, question, feature and bug about the plugin system
find docs -path "*extensibility/plugin-system*"

# same, narrowed to decisions
find docs/adr -path "*extensibility*"

# everything referencing a specific item, anywhere in the repository
grep -rn "Q-006" .

# all open questions, with their topic paths
grep -n "^| Q-" docs/questions/INDEX.md
```

If the first level were free-form, plugin material would end up under
`plugins/`, `plugin-system/`, `extensibility/` and `abi/` depending on who filed
it, and no single search would find it. That matters most for agents, which
cannot browse a tree the way a person can — they search.

**To add a first-level topic:** add it to this table, and use it consistently
from then on. The friction is deliberate; the list is only useful while stable.

## How the categories connect

They are not six separate piles. They are one pipeline:

```
        ┌─────────────────────────────────────────────────┐
        ▼                                                 │
   questions/  ──needs digging──▶  research/              │
        │                             │                   │
        │ ◀────────── answers ────────┘                   │
        ▼ decided                                         │
      adr/  ───────▶  planning/  ───────▶  features/      │
                     how to build it      the work        │
                                              │           │
                                              ▼ built     │
                                            bugs/  ───────┘
                                        reveals a design flaw
```

ADR 0007 — `units` moving from L2 to L1 — went the whole way round this loop
before a single line of code existed. That is the system working.

## Four rules, every category

**1. The index is the authority.** If an item is not in its `INDEX.md`, it does
not exist. This matters *more* with nesting, not less: once folders are four deep
you cannot find things by browsing, so the index is the only reliable map.

**2. One line until it earns more.** Every tracked item starts as a row in the
index. It gets a file when there is something that will not fit on the line, and
a folder when it has several artifacts.

```
row in INDEX.md   →   <topic path>/ID-name.md   →   <topic path>/ID-name/
```

**Do not create empty template files.** They look like documentation and contain
nothing.

**3. Nothing is deleted.** Dropped features, fixed bugs, superseded research and
answered questions all stay, with a terminal status. The record of what we used
to think — and why we changed — is what stops decisions being re-litigated.

**4. IDs are never reused.** `B-004` means one bug forever, even if it turned out
to be invalid.

## ID scheme

| Prefix | Category |
|---|---|
| `ADR NNNN` | decisions |
| `R-NNN` | research |
| `F-NNN` | features |
| `B-NNN` | bugs |
| `Q-NNN` | questions |

IDs are **global within a category and independent of where the file sits**, so
moving an item between topics never renumbers it. `grep -rn "Q-006"` finds every
reference across the repository.

Design notes carry no ID — they are named for what they contain and linked by
path.
