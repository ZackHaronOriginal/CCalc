# Design notes

How something works, or how we intend to build it. The category for material that
is not a decision, not research, not a tracked feature, and not a defect — plans,
sketches, interface notes, and documentation of how existing code behaves.

[`INDEX.md`](INDEX.md) lists every note.

## What belongs here

- **Plans** — "how we are going to do lexical handling", written before the code
- **Interface notes** — the shape of a type or an API, and why it is shaped that way
- **Code documentation** — how a subsystem actually works, once it does
- **Sketches** — half-formed thinking that is worth keeping but is not a decision

## What does not

| If it is… | It goes in |
|---|---|
| a choice with alternatives rejected | `adr/` |
| an investigation with findings | `research/` |
| work we intend to do | `features/` |
| something broken | `bugs/` |
| something undecided | `questions/` |

A design note that hardens into a choice should become an ADR. Leaving a real
decision buried in a design note means nobody can find it later.

## Structure

Nested topic folders, as deep as the subject needs, with as many notes per folder
as it takes:

```
design/
└── expression-engine/          topic
    └── lexical-handling/       subtopic
        ├── tokenizer-plan.md
        ├── number-literal-forms.md
        └── unicode-identifiers.md
```

Notes here do not carry IDs — they are named for what they contain. Link to them
from features, ADRs and questions by path.
