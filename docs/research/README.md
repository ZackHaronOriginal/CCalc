# Research

Where findings live. When an agent or a person investigates something, the result
goes here — not in a chat log that disappears, and not in someone's memory.

Filed in nested topic folders — see [`../README.md`](../README.md) for the shared
conventions. [`INDEX.md`](INDEX.md) lists every entry.

## Layout

Research is the one category where **each entry gets its own folder**, because
real research produces more than one artifact: findings, sources, measurements,
code samples.

```
research/
└── extensibility/
    └── plugin-system/
        ├── abi/
        │   └── R-001-cpp-abi-stability/
        │       ├── README.md      ← the findings. this is the deliverable.
        │       └── sources.md     ← where each claim came from
        └── discovery/
            └── R-003-linux-plugin-discovery/
                ├── README.md
                └── sources.md
```

## What an entry must contain

**`README.md` is the deliverable and must be readable without opening anything
else.** It answers:

1. **The question** — what were we trying to find out?
2. **The answer** — the conclusion, stated up front, in plain language
3. **What it means for CCalc** — the actionable part. Findings with no
   consequence for this project are trivia.
4. **What we rejected** — approaches dismissed, with reasons
5. **Confidence and gaps** — what is solid, what is assumed, what is untested

**`sources.md`** lists where each claim came from, so a future reader can check
it rather than trusting us.

## Rules

- **Write the conclusion first.** A file that opens with methodology and buries
  the answer on page two will not be read, which makes the research worthless
  however good it was.
- **Record what was rejected.** Half the value is knowing which
  promising-looking road is a dead end.
- **Be explicit about confidence.** "Verified by building it" and "the docs claim
  this" are very different, and a reader cannot tell them apart later.
- **Never delete a superseded entry.** Mark it `superseded`, say what replaced it.
- **Link outward.** Research that produced a decision links the ADR; research that
  produced work links the feature.

Research that concludes nothing and changes nothing is not finished.
