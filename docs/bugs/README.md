# Bugs

Known defects. Filed by topic ([`../topics/INDEX.md`](../topics/INDEX.md)).
[`INDEX.md`](INDEX.md) is the bug list — every bug, one line each.

## The escalation ladder

**A bug starts as a single row in `INDEX.md` and gets a file only when it needs
one.**

```
one line in INDEX.md   →   <topic>/B-NNN-name.md   →   <topic>/B-NNN-name/
   most bugs                 needs investigation        needs repro data
```

Most bugs are one line: what is wrong, where. Write a file when there is
something worth keeping — a reproduction that took effort to find, a wrong
theory that was ruled out, a decision to accept the behaviour.

## Status values

| Status | Meaning |
|---|---|
| `open` | reported, not yet reproduced |
| `confirmed` | reproduced; we know it is real |
| `fixed` | corrected, with a regression test |
| `wontfix` | real, but we have chosen to live with it — **give the reason** |
| `invalid` | not actually a bug — **say why**, so it is not re-reported |

## Rules

- **Every fix ships with a regression test.** A bug with no test will come back,
  and the second time nobody remembers the first.
- **Never delete a fixed or invalid bug.** The row is the record, and it stops
  the same report arriving twice.
- **State how to reproduce it**, even in a one-line entry. `sqrt(-1)` hangs" is
  actionable; "sqrt is broken" is not.
- **Never disable or skip a test to make the build green.** That converts a
  visible bug into an invisible one.
- IDs are never reused.

## Severity

| Level | Meaning |
|---|---|
| `crash` | CCalc terminates, hangs, or corrupts data |
| `wrong` | produces an incorrect answer — worst kind short of a crash, because the user cannot tell |
| `poor` | works, but the behaviour or message is bad |
| `cosmetic` | formatting, alignment, wording |

`wrong` deserves special attention in a calculator. A crash is obvious; a
confidently wrong number is not, and it is the failure a calculator can least
afford.

## Where bugs come from

```
feature  →  bug  →  question  →  ADR
```

Most bugs are ordinary defects fixed in place. But a bug that reveals a design
flaw should raise a question rather than just being patched — and if the answer
changes how the project works, it becomes an ADR. `units` moving from L2 to L1
(ADR 0007) is exactly that path, caught before any code existed.
