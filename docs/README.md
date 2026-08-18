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
| [`design/`](design/) | plans, interface notes, how things work | [INDEX](design/INDEX.md) |
| [`features/`](features/) | what we intend to build | [INDEX](features/INDEX.md) |
| [`bugs/`](bugs/) | known defects | [INDEX](bugs/INDEX.md) |
| [`questions/`](questions/) | what we have not decided | [INDEX](questions/INDEX.md) |

## How items are filed: nested topic folders

Inside every category, items live in **topic folders that nest as deep as the
subject needs** — typically two to four levels. Any folder at any depth can hold
as many notes as it takes.

```
docs/design/                          ← category
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

Create a subtopic when a folder has enough notes that scanning it is annoying —
not before. A folder with two files does not need a subdirectory.

### Keep the top level consistent across categories

Below the first level, nest however the subject wants. But use the *same*
top-level topic names in every category, so `plugin-system` material is under
`extensibility/` whether it is a decision, a question or a bug.

Currently in use:

`expression-engine` · `foundation` · `mathematics` · `graphing` ·
`extensibility` · `interface` · `project-structure` · `build-and-release`

This is a convention, not a fixed vocabulary. Add a top-level topic when
something genuinely does not fit — just add it everywhere it applies.

## How the categories connect

They are not six separate piles. They are one pipeline:

```
        ┌─────────────────────────────────────────────────┐
        ▼                                                 │
   questions/  ──needs digging──▶  research/              │
        │                             │                   │
        │ ◀────────── answers ────────┘                   │
        ▼ decided                                         │
      adr/  ────────▶  design/  ────────▶  features/      │
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
