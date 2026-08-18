# Research

Where findings live. When an agent or a person investigates a topic, the result
goes here — not in a chat log that disappears, and not in someone's memory.

Filed by topic ([`../topics/INDEX.md`](../topics/INDEX.md)). [`INDEX.md`](INDEX.md) lists
every entry and holds the next free ID.

## Layout

Research is the one area where **each entry gets its own folder**, because real
research produces more than one artifact: findings, sources, code samples,
measurements.

```
research/
├── INDEX.md
├── 0000-template.md
├── plugins/
│   └── R-001-cpp-abi-stability/
│       ├── README.md      ← the findings. this is the deliverable.
│       └── sources.md     ← where each claim came from
└── project/
    └── R-002-cpp-project-layout-standards/
        ├── README.md
        └── sources.md
```

## What a research entry must contain

**`README.md` is the deliverable, and it must be readable without opening
anything else.** It answers:

1. **The question** — what were we actually trying to find out?
2. **The answer** — the conclusion, stated up front, in plain language
3. **What it means for CCalc** — the actionable part. Findings with no
   consequence for this project are trivia.
4. **What we rejected** — approaches considered and dismissed, with reasons
5. **Confidence and gaps** — what is solid, what is assumption, what is untested

**`sources.md`** lists where each claim came from, so a future reader can check
it rather than trusting us.

## Rules

- **Write the conclusion first.** A research file that opens with methodology and
  buries the answer on page two will not be read, which makes the research
  worthless no matter how good it was.
- **Record what was rejected.** Half the value of research is knowing which
  promising-looking road is a dead end. Without it, someone walks down it again.
- **Be explicit about confidence.** "Verified by building it" and "the docs claim
  this" are very different, and a reader cannot tell them apart later.
- **Never delete a superseded entry.** Mark it `superseded`, say what replaced it.
  Knowing what we used to believe explains decisions that look odd now.
- **Link outward.** If research produced a decision, link the ADR. If it produced
  work, link the feature.

## Where research fits

```
question (questions/)  →  research/  →  ADR  →  feature
```

Research is what you do when a question cannot be answered from what we already
know. It ends by answering the question — which usually means writing an ADR.
Research that concludes nothing and changes nothing was not finished.
