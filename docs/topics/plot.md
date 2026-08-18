# plot

**Layer:** L3 · **Module:** [`libs/plot/`](../../libs/plot/README.md)

Working out **what** a graph should look like. Never how it is drawn.

## The rule

`plot` returns pure data: sample points, axis crossings, tick positions and
labels, clipping and break information. No colours, no pixels, no window, and
**no UI toolkit header anywhere in `libs/`**.

Frontends render it — `cli` with Unicode blocks, `gui` with a canvas. Same data,
different renderers.

## In scope

Adaptive sampling, linear and log scales, tick-interval selection, asymptote
detection, the `PlotData` type. Later: polar, parametric, 3D, multiple series.

## Not in scope

All drawing. Colours, fonts, line widths, themes — those are rendering choices
belonging to `cli` or `gui`.

## Telling it apart

| Confusable with | Rule |
|---|---|
| `cli` / `gui` | Deciding a tick goes at 0.2 → `plot`. Deciding it is drawn as `┼` → the frontend |
| `eval` | `plot` calls `eval` to compute `f(x)`; that direction is correct and never reversed |

**Worked example:** "ticks show 0.142857 instead of 0.2" is `plot`. "ticks are
misaligned by one column in the terminal" is `cli`.

## Decisions in force

ADR 0005 — geometry only, never renders.
