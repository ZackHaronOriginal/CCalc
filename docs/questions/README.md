# Questions

Things we have not decided.

Filed in nested topic folders — see [`../README.md`](../README.md) for the shared
conventions. [`INDEX.md`](INDEX.md) is the list.

## The escalation ladder

```
row in INDEX.md   →   <topic path>/Q-NNN-name.md   →   ../research/
   most questions       real trade-offs to weigh       needs investigation
```

A question that needs a *file* is one where the options and their consequences
are worth writing down. A question that needs **research** has outgrown this
category — open a research entry, link it, and keep the question open until the
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
and a one-line answer with a link is far cheaper than rediscovering the reason.

## Rules

- **Say what it blocks.** A question nothing depends on is not urgent, and saying
  so is useful. A question blocking three features needs deciding now.
- **Say when it must be decided.** Some questions are cheap to defer forever;
  some become irreversible at a specific moment.
- **Do not settle a real decision here.** Write the ADR. A question file records
  the *options*; an ADR records the *choice* and why the alternatives lost.
- IDs are never reused.

## Where questions fit

```
questions → research → adr → design → features
```

Questions are the entry point to the whole pipeline. Most decisions in this
project should start here.
