# plot

**Layer L3** · target `ccalc_plot` · namespace `ccalc::plot` · `<ccalc/plot/…>`

Works out **what** a graph should look like. It never draws anything.

## The one rule

> This module computes geometry. It does not render.

Given `sin(x)` from −10 to 10 in an 800×600 area, it returns: sample points,
where the axes cross, where the tick marks go and what their labels say, and
which parts of the curve fall outside the view. Pure data. No colours, no pixels,
no window, no toolkit.

The frontends then draw that data — `apps/gui` with a real canvas, `apps/cli`
with Unicode block characters in the terminal. **Same module, same data, two
renderers.**

## What goes here

- `Sampler` — evaluating a function across a range, sampling more densely where
  the curve bends sharply
- `Scale` — mapping mathematical coordinates to view coordinates, linear and
  logarithmic
- `Ticks` — choosing human-friendly tick intervals
- `Asymptote` detection — finding where a function shoots to infinity so the
  renderer draws a break instead of a near-vertical line
- `PlotData` — the output type handed to a renderer
- Later: polar and parametric plots, multiple series, 3D surfaces

## What does NOT go here

- **Any drawing code.** No Qt, no GTK, no Cairo, no terminal escape codes.
- Colours, fonts, line widths, themes. Those are rendering choices.
- Anything that includes a UI toolkit header. If a toolkit header appears in
  this directory, the boundary has been broken.

## Why this is the most valuable boundary in the project

Graphing is where the hardest mathematics and the heaviest UI code meet. Keeping
them apart means:

- The hard parts are unit-testable with plain numbers. Ask for a range of 0 to 1
  and assert the ticks come back as `0, 0.2, 0.4, 0.6, 0.8, 1.0` — not
  `0, 0.142857, 0.285714`. Choosing round tick values is a surprisingly fiddly
  algorithm, and if drawing and computing were tangled together that test would
  need a graphics context, so it would never get written.
- Terminal plots come almost free.
- Switching UI toolkits touches `apps/gui` only.
- PNG and SVG export later is another renderer, not a rewrite.
- A plugin can add a new plot type without touching any rendering code.

Graphing code welded to a UI toolkit is the single most common reason calculator
projects stop being extensible. This boundary is how we avoid that.

## Layer note

This module uses `eval` to compute `f(x)`, which points downward and is correct.
`eval` must never call back into here — see `libs/eval/README.md`.
