# Architecture Decision Records

One short file per significant decision: what was decided, what else was
considered, and **why**.

Filed in nested topic folders — see [`../README.md`](../README.md) for the shared
conventions. [`INDEX.md`](INDEX.md) lists every record and holds the next free
number.

## Why bother

Six months from now, someone — possibly you — will look at a choice here and
think "that seems arbitrary, let me simplify it." Without a written reason they
will either re-litigate a settled question or quietly reverse it and rediscover
the original problem the hard way.

An ADR is insurance against that. It costs ten minutes.

## Layout

```
adr/
├── INDEX.md
├── 0000-template.md
├── expression-engine/notation-handling/   0002
├── extensibility/plugin-system/abi/       0006
├── foundation/error-handling/             0003
├── graphing/geometry/                     0005
├── mathematics/unit-handling/             0007
└── project-structure/module-layout/       0001, 0004
```

**Numbers are global, not per folder.** ADR 0004 is one document wherever it
lives, and moving it between topics never renumbers it. The cost is that you
cannot find the next free number by looking in one place — which is exactly what
`INDEX.md` is for.

## How to add one

1. Check `INDEX.md` for the next free number
2. Copy `0000-template.md` to `<topic>/<subtopic>/NNNN-short-title.md`
3. Fill it in — **including the alternatives you rejected**
4. Add a row to `INDEX.md`
5. Commit it with the change it describes

## Rules

- **Never delete or rewrite an accepted ADR.** If a decision is reversed, write a
  new one that supersedes it and mark the old one `Superseded by NNNN`.
- Keep them short. A page is plenty.
- **Always record the alternatives.** An ADR without them says what we do but not
  why the obvious other option is worse — and that is exactly what someone will
  propose in a year.
- Never reuse a number.

## What deserves an ADR

Anything hard to reverse, or where a reasonable person would choose differently:
module boundaries and layers, the plugin ABI, dependency choices, file formats,
the UI toolkit.

Not: naming a variable, splitting a long function, ordinary refactors. And not a
plan for how to build something — that is a planning note.
