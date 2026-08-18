# Q-009. How does a user interrupt a long computation?

**Topic:** expression-engine / evaluation · **Status:** open
**Raised:** 2026-08-18 · **Blocks:** F-002, F-013, F-017
**Decide by:** M1 — it is part of the evaluator's signature

## The question

A calculator can be asked to do something that takes a very long time — a
250,000-digit factorial, an adaptive plot of a pathological function, a plugin
that loops. What lets the user stop it?

## Why it matters

Without an answer, Ctrl-C kills the process and the user loses their session
variables and history. In a GUI it is worse: the window freezes with no way out.

This is not a feature that can be bolted on. **Interruption has to be visible in
the evaluator's signature**, because it means every evaluation must be able to
return "stopped" — which changes the return type everywhere. Retrofitting it
touches every call site in `eval`, `functions` and `plot`.

## Options

### Option A — no cancellation

Nothing to build. The user kills the process and loses everything. Acceptable for
M1; not acceptable once graphing exists.

### Option B — a cancellation token passed through evaluation

Evaluation takes a token it polls at intervals (loop iterations, node counts).
Cancelling sets a flag; evaluation returns a "cancelled" diagnostic.

Explicit, testable, no threads required. But the token has to be threaded through
every signature — and **through the plugin ABI**, which is why it interacts with
Q-006 and must be settled before ABI v1 freezes.

### Option C — an operation budget

Instead of an external signal, evaluation carries a limit — node visits, or
elapsed time — and gives up when exceeded.

Simpler and needs no external actor. But a budget cannot distinguish "the user
changed their mind" from "this is genuinely slow", and any fixed limit is wrong
for someone.

### Option D — evaluate on a worker thread and abandon it

The GUI's natural approach, but abandoning a thread mid-computation leaks
whatever it allocated, and killing one running plugin code is not safe.

## What would settle it

Two things worth checking: how granular the polling has to be before it costs
measurable throughput on the graphing path, and whether the plugin ABI can carry
a cancellation check without making plugin authors handle something easy to
forget.

## Cost of deferring

**The mechanism can wait. The signature cannot.**

Decide in M1 only whether evaluation returns a type that can express "did not
finish". If yes, the mechanism can arrive at M6 with no rework. If no, adding it
later is a change to every signature in three modules — plus a frozen plugin ABI
that has no room for it.
