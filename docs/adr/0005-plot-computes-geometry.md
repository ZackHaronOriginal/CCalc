# 5. plot computes geometry and never renders

**Status:** Accepted
**Date:** 2026-08-18

## Context

Graphing is where the hardest mathematics in CCalc — adaptive sampling, choosing
round tick intervals, detecting asymptotes — meets the heaviest UI code. We plan
at least two frontends (terminal and desktop) and possibly image export later.

The default approach, drawing directly from the plotting code, welds the two
together.

## Decision

`libs/plot` computes **what** should appear and returns it as plain data: sample
points, axis crossings, tick positions and labels, clipping information. It
contains no drawing code, no colours, and no UI toolkit headers.

Frontends render that data. `apps/gui` draws it on a canvas; `apps/cli` draws it
with Unicode block characters.

**No UI toolkit header may appear anywhere in `libs/`.**

## Alternatives considered

**Draw directly in the plotting module.** Less code, and the obvious approach.
Rejected because it makes the difficult maths untestable without a graphics
context — meaning the tick-interval algorithm, which is exactly the kind of
fiddly code that needs tests, would never get one.

**An abstract renderer interface that `plot` calls.** Tempting, and it does keep
toolkits out of `libs/`. Rejected as the wrong shape: it inverts control so the
frontend cannot decide *when* or *how often* to draw, and it forces every
renderer into one drawing model. Returning data leaves the frontend in charge.

## Consequences

Tick selection is testable by asserting that the range 0 to 1 yields
`0, 0.2, 0.4, 0.6, 0.8, 1.0` rather than `0, 0.142857, …` — with no window open.
Terminal plots come nearly free. The UI toolkit choice stays reversible, which
is why ADR 0008 (toolkit selection) can be deferred without cost. PNG and SVG
export later is another renderer, not a rewrite.

The costs: the `PlotData` format must be designed carefully and early, since
everything renders from it; and some drawing-time optimisations become harder,
because the geometry layer cannot know what a particular renderer finds cheap.
