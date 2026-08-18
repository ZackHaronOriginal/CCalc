# docs/

Two different things live here, with different lifecycles. Keeping them apart is
what stops either from rotting.

## Reference — what we tell people

Curated, kept current, describes how CCalc *is*.

| File | Contents |
|---|---|
| `ARCHITECTURE.md` | module layers, the dependency rule, the reasoning |
| `GRAMMAR.md` | the formal expression grammar *(to be written)* |
| `PLUGINS.md` | the guide for plugin authors *(to be written)*  |
| `manual/` | the end-user manual *(to be written)* |

**Rule:** if you change the thing, you change its document in the same commit.
Documentation that drifts is worse than none, because people trust it.

## Knowledge base — how we work

Accumulates, is never rewritten, describes how CCalc *came to be*.

| Area | Contents | Index |
|---|---|---|
| [`adr/`](adr/) | decisions, and why the alternatives lost | [INDEX](adr/INDEX.md) |
| [`research/`](research/) | findings from investigation | [INDEX](research/INDEX.md) |
| [`features/`](features/) | what we intend to build | [INDEX](features/INDEX.md) |
| [`bugs/`](bugs/) | known defects | [INDEX](bugs/INDEX.md) |
| [`QUESTIONS.md`](QUESTIONS.md) | what we have not decided | *(the file is the index)* |

[`TOPICS.md`](TOPICS.md) is the shared topic list every area files under.

## How the areas connect

They are not four separate piles. They are one pipeline:

```
        ┌──────────────────────────────────────────────┐
        │                                              │
        ▼                                              │
   QUESTIONS.md  ──needs digging──▶  research/         │
        │                               │              │
        │ ◀────────── answers ──────────┘              │
        │                                              │
        ▼ decided                                      │
      adr/  ──────implies work──────▶  features/       │
                                          │            │
                                          ▼ built      │
                                        bugs/  ────────┘
                                            reveals a design flaw
```

- A **question** we cannot answer from what we know becomes **research**
- **Research** that concludes becomes a **decision** (ADR)
- A **decision** that implies work becomes a **feature**
- A **feature**, once built, may produce **bugs**
- A **bug** that exposes a design flaw raises a new **question**

ADR 0007 — `units` moving from L2 to L1 — went the whole way round this loop
before a single line of code existed. That is the system working.

## Four rules that apply to every area

**1. The index is the authority.** If an item is not in its `INDEX.md`, it does
not exist. This is what prevents orphaned files nobody can find.

**2. One line until it earns more.** Every item starts as a single row. It gets
its own file when there is something to say that will not fit on the line, and a
folder only when it has several artifacts. An empty template file for every idea
is worse than no file — it looks like documentation and contains nothing.

```
row in INDEX.md   →   <topic>/ID-name.md   →   <topic>/ID-name/
```

**3. Nothing is deleted.** Dropped features, fixed bugs, superseded research and
answered questions all stay, with a terminal status. The record of what we used
to think — and why we changed — is the part that stops decisions being
re-litigated every six months.

**4. IDs are never reused.** `B-004` means one bug forever, even if it turned out
to be invalid.

## ID scheme

| Prefix | Area |
|---|---|
| `ADR NNNN` | decisions |
| `R-NNN` | research |
| `F-NNN` | features |
| `B-NNN` | bugs |
| `Q-NNN` | questions |

Every ID is globally unique within its area and greppable across the whole
repository, so `grep -rn "Q-006"` finds every place a question is referenced.
