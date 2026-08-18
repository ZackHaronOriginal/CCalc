# Features

What we intend to build, and what we have decided not to.

Filed by topic ([`../TOPICS.md`](../TOPICS.md)). [`INDEX.md`](INDEX.md) is the
backlog — every feature, one line each.

## The escalation ladder

**A feature starts as a single row in `INDEX.md` and gets a file only when it
needs one.**

```
one line in INDEX.md   →   <topic>/F-NNN-name.md   →   <topic>/F-NNN-name/
      most features          needs real design         needs several artifacts
```

This is the rule that keeps the backlog usable. Fourteen planned features do not
need fourteen design documents — most need one line saying what they are. Write
the file when there is something to say that will not fit on the line: open
questions, a design sketch, a list of sub-tasks, an interface proposal.

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

**Never delete a dropped feature.** The row costs one line and stops the same
idea being proposed every six months. If the reason is interesting, it is
probably an ADR.

## Rules

- **The index is the authority.** If it is not in `INDEX.md`, it is not planned.
- **One line means one line.** ID, topic, summary, status, milestone, link. If
  the summary does not fit, the feature is too vague or too large — split it.
- **Link to the decision.** If an ADR settles how a feature works, link it. If a
  feature needs a decision first, say so in the row.
- **IDs are never reused**, even for dropped features.

## Where features come from

```
question  →  research  →  ADR  →  feature  →  (bugs)
```

A feature is usually the *implementation* of a settled decision. If a feature
still has open design questions, those belong in
[`../QUESTIONS.md`](../QUESTIONS.md) — and the feature row should say it is
blocked on them.
