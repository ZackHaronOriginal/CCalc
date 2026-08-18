# Planning

How we intend to build something, and how it works once built. The category for
material that is not a decision, not research, not a tracked feature, and not a
defect — plans, sketches, interface notes, and documentation of how existing
code behaves.

[`INDEX.md`](INDEX.md) lists every note.

## What belongs here

- **Plans** — "how we are going to do lexical handling", written before the code
- **Interface notes** — the shape of a type or an API, and why it is shaped that way
- **Code documentation** — how a subsystem actually works, once it does
- **Sketches** — half-formed thinking worth keeping but not yet a decision

## What does not

| If it is… | It goes in |
|---|---|
| a choice with alternatives rejected | `adr/` |
| an investigation with findings | `research/` |
| work we intend to do | `features/` |
| something broken | `bugs/` |
| something undecided | `questions/` |

A planning note that hardens into a choice should become an ADR. Leaving a real
decision buried in a planning note means nobody can find it later.

## Structure

Nested topic folders, as deep as the subject needs, with as many notes per folder
as it takes:

```
planning/
└── expression-engine/          first-level topic (fixed list)
    └── lexical-handling/       free-form subtopic
        ├── tokenizer-plan.md
        ├── number-literal-forms.md
        └── unicode-identifiers.md
```

Notes here carry no IDs — they are named for what they contain and linked by
path.

## The status that matters

A planning note describing code as it *used to be* is worse than no note, because
it is confidently wrong. When you change code a note describes, update the note in
the same commit or mark it `stale`. See [`INDEX.md`](INDEX.md) for the values.
