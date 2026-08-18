# Features

What we intend to build, and what we have decided not to.

Filed in nested topic folders — see [`../README.md`](../README.md) for the shared
conventions. [`INDEX.md`](INDEX.md) is the backlog.

## The escalation ladder

**A feature starts as a single row in `INDEX.md` and gets a file only when it
needs one.**

```
row in INDEX.md   →   <topic path>/F-NNN-name.md   →   <topic path>/F-NNN-name/
   most features          needs real design            needs several artifacts
```

Twenty planned features do not need twenty design documents — most need one line
saying what they are. Write the file when there is something that will not fit:
open questions, a design sketch, sub-tasks, an interface proposal.

An empty template file for every idea is worse than no file, because it looks
like documentation and contains nothing.

## Status values

| Status | Meaning |
|---|---|
| `idea` | worth considering; not committed to |
| `planned` | we intend to build it; assigned to a milestone |
| `active` | being built now |
| `done` | shipped |
| `dropped` | decided against — **keep the row**, with the reason |

**Never delete a dropped feature.** One line stops the same idea being proposed
every six months. If the reason is interesting, it is probably an ADR.

## Rules

- **The index is the authority.** Not in `INDEX.md` means not planned.
- **One line means one line.** ID, topic, summary, status, milestone, link. If the
  summary does not fit, the feature is too vague or too large — split it.
- **Link to the decision.** If an ADR settles how a feature works, link it. If a
  feature needs a decision first, say so in the row.
- IDs are never reused, and never change when a file moves between topics.

## Where features come from

```
questions → research → adr → design → features → (bugs)
```

A feature is usually the *implementation* of a settled decision. Open design
questions belong in [`../questions/`](../questions/), and the feature row should
say it is blocked on them. How to build it belongs in
[`../planning/`](../planning/).
