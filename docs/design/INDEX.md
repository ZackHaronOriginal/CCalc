# Design note index

Every design note. **If it is not listed here, it does not exist.**

Design notes carry no IDs — they are named for what they contain and linked by
path.

| Topic path | Note | Status | Related |
|---|---|---|---|
| expression-engine / lexical-handling | [Tokenizer plan](expression-engine/lexical-handling/tokenizer-plan.md) | draft | F-001, ADR 0002 |
| expression-engine / lexical-handling | [Number literal forms](expression-engine/lexical-handling/number-literal-forms.md) | draft | F-001 |
| mathematics / matrix-handling | [Matrix representation notes](mathematics/matrix-handling/representation-notes.md) | draft | F-011, Q-004 |
| extensibility / plugin-system / abi | [ABI struct layout](extensibility/plugin-system/abi/struct-layout.md) | draft | ADR 0006, Q-006 |

## Status values

| Status | Meaning |
|---|---|
| `draft` | thinking in progress; not settled |
| `agreed` | this is how we are doing it |
| `built` | the code matches this; the note documents reality |
| `stale` | the code moved on — **fix or mark it**, never leave it silently wrong |

`stale` is the important one. A design note that describes code as it *used to
be* is worse than no note, because it is confidently wrong. When you change code
that a note describes, update the note in the same commit or mark it stale.
