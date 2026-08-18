# gui

**Layer:** app · **Directory:** [`apps/gui/`](../../apps/gui/README.md)

The desktop frontend. **Not started** — the folder exists so the boundary is
respected from the first line of code.

## In scope

Main window, expression entry, results view, variable panel. The graph canvas
rendering `PlotData`. Pan, zoom, hover readout. Menus, preferences, shortcuts.
Views over `session` history and workspaces.

## Not in scope

Any calculation, ever. Any plot geometry — it receives sample points and tick
positions and draws them.

## The rule that protects everything else

> **UI toolkit headers never leave `apps/gui/`.**

No `#include <QtWidgets>` anywhere in `libs/`. The moment a toolkit header
appears in a library module, switching toolkits becomes impossible and the maths
becomes untestable without a display.

## Open

Q-001 — Qt or GTK. Deliberately deferred: because `plot` returns pure geometry,
this choice stays cheap and reversible until M8. That deferral is exactly what
the boundary buys us, so there is no reason to decide early.
