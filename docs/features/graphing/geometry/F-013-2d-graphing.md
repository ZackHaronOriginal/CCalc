# F-013. 2D function graphing

**Topic:** `plot` · **Status:** planned
**Milestone:** M6 · **Related:** ADR 0005, F-014, F-017

## What

Given an expression and a range, produce everything needed to draw its graph:
sample points, axis positions, tick marks with round-number labels, and the
information a renderer needs to break the curve at asymptotes.

The user types `plot(sin(x), -10, 10)` and sees a graph — in the terminal
(F-014) or in the desktop app (F-017), from the same computation.

## Why

Graphing is the headline feature separating CCalc from the simple calculators it
is meant to beat. It is also the hardest thing in the project, which is why it
gets a design file this early.

## Scope

**In:** adaptive sampling; linear and logarithmic scales; tick interval
selection; asymptote and discontinuity detection; clipping; the `PlotData` output
type.

**Out:** all drawing. No colours, no pixels, no toolkit — see ADR 0005. Also out:
polar and parametric plots (F-020), 3D surfaces, and multiple series, which come
later on top of this.

## Design notes

Three genuinely hard sub-problems, each worth its own research before coding:

**Tick selection.** Asking for the range 0 to 1 must yield `0, 0.2, 0.4, 0.6,
0.8, 1.0` — not `0, 0.142857, 0.285714`. Humans read round numbers. This is
fiddlier than it looks; Wilkinson's extended algorithm is the known-good
approach, and a simpler nice-numbers method may be enough. Research needed.

**Adaptive sampling.** Uniform sampling wastes effort on straight sections and
misses detail where the curve bends sharply. Sample density should follow
curvature.

**Asymptotes.** `tan(x)` shoots to infinity at regular intervals. Without
detection, the renderer draws a near-vertical line joining `+∞` to `−∞`, which is
visually wrong and looks like a bug. The geometry layer must mark the break so
every renderer handles it consistently.

`PlotData` is the contract every renderer reads, so it must be designed
carefully and early — changing it later touches every frontend.

## Open questions

- Which tick algorithm? Blocked on research (see `../../research/INDEX.md`).
- How does `PlotData` represent a break — a sentinel value, or explicit segments?
  Segments are cleaner for renderers; sentinels are cheaper to produce.

## Done when

- Range 0 to 1 produces exactly the ticks `0, 0.2, 0.4, 0.6, 0.8, 1.0`
- `tan(x)` over `-10..10` produces marked breaks, not vertical lines
- Sampling is denser near sharp bends than on straight sections, measurably
- Every one of the above is unit-tested **with no graphics context and no window**
- `libs/plot/` contains no UI toolkit header of any kind
