# Q-001. Qt or GTK for the desktop frontend?

**Topic:** `gui` · **Status:** open
**Raised:** 2026-08-18 · **Blocks:** F-017
**Decide by:** M8 — and deliberately cheap to defer until then

## The question

CCalc will have a desktop frontend with a graph canvas. The two realistic choices
on Linux are Qt and GTK.

## Why it matters

It affects licensing, package size, how good the graph canvas can be, and how
native the app feels on each desktop environment. It is also the largest external
dependency the project will take on.

## Options

### Option A — Qt

Stronger canvas story: `QGraphicsView` and `QPainter` are mature, well documented
and handle the interaction we want (pan, zoom, hover readout) without much
scaffolding. Better cross-platform reach if CCalc ever leaves Linux. Larger
install footprint, and the licensing needs care — LGPL is workable but constrains
static linking.

### Option B — GTK

Integrates more naturally on GNOME, which is the default on several major
distributions. Simpler licensing (LGPL, but in practice less friction for this
kind of app). Cairo is capable for our drawing needs. Weaker on Windows and
macOS, and the C API is more awkward from C++ — `gtkmm` helps but adds a layer.

## What would settle it

Research covering: licensing implications for each packaging target, install
size, canvas quality for interactive plots specifically, and how each behaves
under Flatpak.

A small prototype rendering a `PlotData` with pan and zoom in each toolkit would
answer the canvas question definitively, and is probably a day's work.

## Cost of deferring

**Near zero, and this is by design.** ADR 0005 keeps `libs/plot` returning pure
geometry with no toolkit headers anywhere in `libs/`. The engine does not know a
GUI exists, so the choice touches `apps/gui/` and nothing else.

Deciding early would gain nothing and risks committing before we know what the
canvas actually needs to do. This question stays open on purpose.

Record the decision as an ADR when it is made, including whichever option loses
and why.
