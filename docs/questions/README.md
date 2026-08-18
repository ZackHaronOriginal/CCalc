# Questions

Things we have not decided. [`INDEX.md`](INDEX.md) is the list — one line each.
Detail files live in topic folders here, for questions that need more than a line.

Filed by topic ([`../topics/INDEX.md`](../topics/INDEX.md)).

## The escalation ladder

```
row in INDEX.md   →   <topic>/Q-NNN-name.md   →   research/
   most questions        real trade-offs to weigh    needs investigation
```

A question that needs a *file* is one where the options and their consequences
are worth writing down. A question that needs **research** has outgrown this area
entirely — open a research entry, link it, and keep the question open until the
research concludes.

## How a question ends

A question is **closed, never deleted.**

| Ending | What happens |
|---|---|
| **Decided** | becomes an ADR — move to Answered with the link |
| **Needs investigation** | becomes research — link it, keep the question open |
| **Answered trivially** | record the answer inline, move to Answered |
| **No longer relevant** | move to Answered with the reason it stopped mattering |

Keeping closed questions matters. "Why don't we use GMP?" gets asked repeatedly,
and a one-line answer with a link is far cheaper than rediscovering the reason
from scratch.

## Rules

- **Say what it blocks.** A question nothing depends on is not urgent, and saying
  so is useful. A question blocking three features needs deciding now.
- **Say when it must be decided.** Some questions are cheap to defer (Q-001,
  the UI toolkit) and some get expensive fast (Q-006, arrays in ABI v1 — after
  publication it is too late).
- **Do not settle a real decision here.** Write the ADR. A question file records
  the *options*; an ADR records the *choice* and why the alternatives lost.
- IDs are never reused.

## Where questions fit

```
QUESTIONS ──needs digging──▶ research ──concludes──▶ ADR ──implies work──▶ features
```

Questions are the entry point to the whole pipeline. Most decisions in this
project should start here.
