# Bugs

Known defects.

Filed in nested topic folders — see [`../README.md`](../README.md) for the shared
conventions. [`INDEX.md`](INDEX.md) is the bug list.

## The escalation ladder

```
row in INDEX.md   →   <topic path>/B-NNN-name.md   →   <topic path>/B-NNN-name/
   most bugs             needs investigation             needs repro data
```

Most bugs are one line: what is wrong, where. Write a file when there is
something worth keeping — a reproduction that took effort to find, a wrong theory
that was ruled out, a decision to accept the behaviour.

## Status values

| Status | Meaning |
|---|---|
| `open` | reported, not yet reproduced |
| `confirmed` | reproduced; we know it is real |
| `fixed` | corrected, with a regression test |
| `wontfix` | real, but we chose to live with it — **give the reason** |
| `invalid` | not actually a bug — **say why**, so it is not re-reported |

## Severity

| Level | Meaning |
|---|---|
| `crash` | CCalc terminates, hangs, or corrupts data |
| `wrong` | produces an incorrect answer |
| `poor` | works, but the behaviour or message is bad |
| `cosmetic` | formatting, alignment, wording |

**`wrong` deserves special weight in a calculator.** A crash is obvious to the
user; a confidently incorrect number is not, and that is the failure a calculator
can least afford.

## Rules

- **Every fix ships with a regression test.** A bug with no test comes back, and
  the second time nobody remembers the first.
- **Never delete a fixed or invalid bug.** The row is the record, and it stops the
  same report arriving twice.
- **State how to reproduce it**, even in one line. "`sqrt(-1)` hangs" is
  actionable; "sqrt is broken" is not.
- **Never disable or skip a test to make the build green.** That converts a
  visible bug into an invisible one.
- IDs are never reused.

## Where bugs come from

```
features → bugs → questions → adr
```

Most bugs are ordinary defects fixed in place. But a bug that exposes a design
flaw should raise a question rather than just being patched — and if the answer
changes how the project works, it becomes an ADR.
