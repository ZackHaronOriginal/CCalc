# 8. Structure the knowledge base as six categories of nested topic folders

**Status:** Accepted
**Date:** 2026-08-18

## Context

Work on CCalc produces material worth keeping that is not code: research
findings, decisions and their reasoning, plans for how to build something, a
backlog, defects, and open questions. Much of it is produced by agents in
sessions that end — if it is not written into the repository it is simply lost,
and the same investigation gets repeated.

An early attempt filed everything under a fixed vocabulary of thirteen topic
names mirroring the `libs/` modules, one level deep. That proved too rigid.
Notes on the tokenizer, on number literal forms, and on Unicode identifiers all
collapsed into `expr` with nowhere to subdivide, and a fixed list cannot name
subjects like "notation handling" or "matrix representation" that do not
correspond to a module.

## Decision

Six **categories** under `docs/`, each holding one kind of thing:

| Category | Holds |
|---|---|
| `research/` | findings from investigation |
| `adr/` | decisions, and why the alternatives lost |
| `planning/` | plans, interface notes, how things work |
| `features/` | what we intend to build |
| `bugs/` | known defects |
| `questions/` | what we have not decided |

Inside each, items are filed in **topic folders that nest as deep as the subject
needs** — typically two to four levels. Topic names below the first level are
free-form and descriptive. Any folder at any depth may hold as many notes as it
takes.

**The first level is a fixed list**, identical across all six categories:

`expression-engine` · `foundation` · `mathematics` · `graphing` ·
`extensibility` · `interface` · `project-structure` · `build-and-release`

Four rules apply everywhere: the `INDEX.md` is authoritative, an item stays one
line until it earns a file and a file until it earns a folder, nothing is ever
deleted, and IDs are never reused or renumbered.

## Alternatives considered

**A fixed topic vocabulary, one level deep** — what we tried first. Rejected:
subjects are finer-grained than modules, and a flat namespace gives a topic with
twenty notes nowhere to go.

**Free-form nesting at every level, including the first.** Rejected because it
destroys cross-category lookup. With a fixed first level,
`find docs -path "*extensibility/plugin-system*"` returns the decisions, the
research, the plan, the features and the open questions together. Without it,
plugin material would sit under `plugins/`, `plugin-system/`, `extensibility/`
and `abi/` depending on who filed it, and no single search would find it.

**One flat `notes/` folder.** Rejected: notes accumulate, nobody deletes them,
and within a year it is a pile of confidently-worded files describing a project
that no longer exists — actively misleading because it looks authoritative.

**GitHub issues and the wiki instead of files in the repository.** Good for work
in flight, and we should still use issues for that. Rejected as the primary
store because it leaves the repository: an agent working in a checkout cannot
read it, it is not versioned with the code that it describes, and it cannot be
reviewed in the same commit as the change it explains.

**Deriving structure from tags in file front-matter rather than folders.** More
flexible in principle. Rejected as requiring tooling we do not have; folders and
`grep` work everywhere, with nothing to build or maintain.

## Consequences

Findings survive the session that produced them. A subject can grow to any depth
without reorganising. The fixed first level makes one search find everything
about a subject across all six categories, which matters most for agents, who
cannot browse.

The costs are real:

- **Browsing stops working.** At four levels deep you cannot find things by
  looking, which makes each `INDEX.md` load-bearing rather than a convenience.
  An item missing from its index is effectively lost — hence rule one.
- **Ongoing discipline.** Every item costs an index update in the same commit.
- **Judgement at the boundaries.** "Is this a planning note or an ADR?" has no
  mechanical answer. The rule of thumb: if alternatives were weighed and rejected,
  it is a decision.
- **The first-level list needs maintaining.** Adding to it means adding
  everywhere it applies, which is deliberate friction — the list is only useful
  while it is stable.

### Relationship to ADR 0001

ADR 0001 forbids nesting in `libs/`; this ADR requires it in `docs/`. That is
not a contradiction. A module has a build target and a name mirrored in four
places, so nesting it breaks that mapping and creates ambiguity. A note has
neither — it is a file, and hierarchy is simply useful. ADR 0001 now carries a
scope clarification saying so explicitly.
