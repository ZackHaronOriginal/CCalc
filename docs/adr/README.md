# Architecture Decision Records

One short file per significant decision: what was decided, what else was
considered, and **why**.

Filed by topic — see [`../topics/INDEX.md`](../topics/INDEX.md) for the list.
[`INDEX.md`](INDEX.md) lists every record and holds the next free number.

## Why bother

Six months from now, someone — possibly you — will look at a choice here and
think "that seems arbitrary, let me simplify it." Without a written reason, they
will either re-litigate a settled question or quietly reverse it and rediscover
the original problem the hard way.

An ADR is insurance against that. It costs ten minutes to write.

## Layout

```
adr/
├── INDEX.md              every record, and the next free number
├── 0000-template.md
├── core/     0003
├── expr/     0002
├── plot/     0005
├── plugins/  0006
├── project/  0001, 0004
└── units/    0007
```

**Numbers are global, not per topic.** ADR 0004 is one document, wherever it
lives. The trade-off is that you cannot find the next free number by looking in
one folder — which is exactly what `INDEX.md` is for. Read it before adding one.

## How to add one

1. Check `INDEX.md` for the next free number
2. Copy `0000-template.md` to `<topic>/NNNN-short-title.md`
3. Fill it in — **including the alternatives you rejected**
4. Add a row to `INDEX.md`
5. Commit it together with the change it describes

## Rules

- **Never delete or rewrite an accepted ADR.** If a decision is reversed, write a
  new one that supersedes it and mark the old one `Superseded by NNNN`.
- Keep them short. A page is plenty.
- **Always record the alternatives.** An ADR without them says what we do but not
  why the obvious other option is worse — and the obvious other option is exactly
  what someone will propose in a year.
- Never reuse a number, even if a record is abandoned.

## What deserves an ADR

Anything hard to reverse, or where a reasonable person would choose differently:
module boundaries and layers, the plugin ABI, dependency choices, file formats,
the UI toolkit.

Not: naming a variable, splitting a long function, ordinary refactors.

## Where ADRs come from

An ADR is usually the *end* of a chain, not the start of one:

```
question (questions/)  →  research/  →  ADR  →  feature
```

A question we cannot answer becomes research. Research that reaches a conclusion
becomes a decision. A decision that implies work becomes a feature. When you
write an ADR, close the question and link the research that fed it.
