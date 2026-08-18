# Q-006. Must ABI version 1 be able to pass arrays?

**Topic:** `plugins` · **Status:** open
**Raised:** 2026-08-18 · **Blocks:** F-015, F-016
**Decide by:** **before ABI v1 is published — irreversible afterwards**

## The question

Statistics functions take a *list* of values: `stddev(1, 2, 3, 4)`. Passing a
list across a C boundary needs a pointer plus a length, and a decision about who
owns the memory.

Does version 1 of the plugin ABI support that, or only scalar arguments?

## Why it matters

This is the most consequential open question in the project, because of *when* it
becomes unanswerable.

Once ABI v1 is published and an external plugin ships against it, the contract is
frozen permanently (ADR 0006). If v1 has no array support, then every
list-taking plugin waits for v2 — and shipping v2 means supporting two ABIs
simultaneously, with two code paths in the host, indefinitely.

A design mistake here is not a refactor. It is permanent.

## Options

### Option A — arrays in v1

The registry signature carries `(const double* values, int32_t count)` alongside
scalars. Ownership rule: the **host** owns the array and it is valid only for the
duration of the call, so the plugin must copy anything it keeps.

More surface to get right immediately, and more to test. But it means the obvious
first thing anyone writes — a statistics pack — works on day one.

### Option B — scalars only in v1

Smaller contract, less to get wrong, ships sooner. But `plugins/stats` (F-016) is
then impossible as a plugin, which removes our own best test of the ABI — and the
whole point of F-016 is to find problems before outsiders do.

### Option C — a generic opaque argument type

A `CCalcArg` struct with a kind tag, covering scalars, arrays and future types
through one mechanism. Most future-proof; also the most to design correctly
before we have real experience with what plugins need.

## What would settle it

**Write `plugins/stats` against a draft ABI before publishing anything.** That is
precisely why F-016 exists. If a real statistics plugin can be written
comfortably, the design works. If it fights the interface, we learn that while
changing it is still free.

Do this before declaring version 1, not after.

## Cost of deferring

**Deferring the *decision* is fine. Deferring past *publication* is fatal.**

There is no cost to leaving this open while the ABI is a draft. There is
unbounded cost to publishing v1 and discovering the answer afterwards.

The rule from ADR 0006 applies exactly here: design for the plugin system from
day one, publish it last.
