# Q-010. Does a `Diagnostic` carry English text, or a message identifier?

**Topic:** foundation / error-handling · **Status:** open
**Raised:** 2026-08-18 · **Blocks:** F-006
**Decide by:** M3 — before `Diagnostic` is widely used

## The question

When the lexer reports `expected a number, found '*'`, does the `Diagnostic`
hold that finished English sentence, or an identifier plus arguments that the
frontend renders?

## Why it matters

`data/translations/` already exists in the layout, so translation is intended.
But **a `Diagnostic` holding a formatted English string cannot be translated
afterwards** without revisiting every place one is constructed — and by M3 that
will be scattered across `expr`, `eval`, `numeric` and `units`.

It also decides whether error text is testable. Asserting on English prose makes
tests break when wording is polished; asserting on an identifier does not.

## Options

### Option A — a formatted string

`Diagnostic { std::string message; Span span; }`

Simplest to write and to read at the point of failure. Translation becomes
impractical later, tests couple to prose, and the same error phrased slightly
differently in two places looks like two errors.

### Option B — an identifier plus arguments

`Diagnostic { MessageId id; SmallVec<Arg> args; Span span; }`

The frontend looks up `id` in a catalogue and substitutes. Translation is then a
data file. Tests assert on `MessageId::ExpectedNumber`, which survives rewording.

Costs a catalogue to maintain, and makes constructing an error slightly more
ceremonious — which is a real tax given how many there will be.

### Option C — identifier plus a fallback string

Carry both: the id for lookup, a plain English fallback for when no catalogue
entry exists.

Pragmatic, and keeps early development fast. Also the one most likely to decay —
if the fallback is always right, nobody notices a missing catalogue entry.

## The plugin dimension

A plugin cannot know our `MessageId` values, so it needs a way to report errors
in its own words. That probably means the ABI carries a plain string *plus* a
generic code, and plugin errors are simply not translatable. Interacts with
ADR 0006 and should be settled before v1 freezes.

## What would settle it

Whether translation is a real goal or an aspiration. If CCalc will only ever ship
in English, Option A is correct and everything else is overhead. That is worth
answering honestly rather than assuming.

## Cost of deferring

**Rising sharply through M3.** Every `Diagnostic` construction site written under
Option A has to be revisited to move to B. There will be dozens by M4 and
hundreds by M6.
