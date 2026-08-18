# ccalc-gui — desktop application

The graphical CCalc. **Not started yet** — this folder exists so the layout is
settled and the boundary is respected from the first line of code.

## What will go here

- The main window: expression entry, results view, variable panel
- The graph view — a canvas that renders `PlotData` from `libs/plot`
- Interaction: pan, zoom, hover readout of coordinates
- Menus, preferences, keyboard shortcuts
- History and workspace views, on top of `libs/session`

## What will NOT go here

- Any calculation, ever. The GUI calls exactly the same `libs/` functions the
  CLI does.
- Any plot geometry. It receives sample points and tick positions and draws them.
  Choosing where a tick goes is `libs/plot`'s job, in every case.

## The rule that protects the rest of the project

> **UI toolkit headers never leave this directory.**

No `#include <QtWidgets>` anywhere in `libs/`. Ever. The moment a toolkit header
appears in a library module, switching toolkits stops being possible and the
maths becomes untestable without a display.

If a toolkit type seems to be needed in a library, that is the signal to convert
it to a plain data type at this boundary instead.

## Toolkit choice — not yet decided

Qt and GTK are both reasonable. The decision should be recorded in
`docs/adr/` when it is made, along with the reasoning.

Deliberately deferring it is itself the point: because `libs/plot` returns pure
geometry and the engine knows nothing about widgets, this choice stays cheap and
reversible for as long as we want. That is exactly what the boundary buys us.
