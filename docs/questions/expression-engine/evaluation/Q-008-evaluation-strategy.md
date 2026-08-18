# Q-008. Walk the tree every time, or compile it once?

**Topic:** expression-engine / evaluation · **Status:** open
**Raised:** 2026-08-18 · **Blocks:** F-002, F-013
**Decide by:** before M6 — and it shapes the AST design in M1

## The question

Does the evaluator walk the syntax tree for every evaluation, or compile the tree
once into a compact form that is then executed repeatedly?

## Why it matters

For a REPL it is irrelevant — one evaluation per keypress, and any approach is
instant.

**Graphing changes that completely.** Plotting `sin(x)` across an 800-pixel-wide
view means evaluating the same expression 800 times, and adaptive sampling
(F-013) pushes that higher. Panning and zooming re-evaluates continuously, so
this sits directly on the interactive path.

A tree walk re-traverses pointers and re-dispatches virtual calls on every
sample. A compiled form does the structural work once.

## Options

### Option A — tree-walking interpreter

The obvious approach: recursive descent over `unique_ptr` nodes, virtual `eval()`
or a visitor.

Simple, easy to debug, and the whole implementation is small. Costs a pointer
chase and an indirect call per node per sample, with poor cache behaviour since
nodes are scattered across the heap.

### Option B — compile to a stack machine

Flatten the tree once into a linear instruction array, then execute that array
per sample. Roughly a hundred extra lines. The execution loop is contiguous
memory with a dense switch — dramatically better cache behaviour, and constant
folding falls out naturally.

Harder to map a runtime error back to a source column, which matters because our
diagnostics are column-precise.

### Option C — tree-walk now, add compilation at M6

Ship A, add B behind the same interface when graphing needs it.

Only viable **if the evaluator interface is designed for it now** — meaning
`plot` asks for something like a `CompiledExpr` rather than being handed a raw
tree. If `plot` takes an AST directly, this option quietly disappears.

## What would settle it

Measure. Build the tree walker in M1, then benchmark 10,000 evaluations of
`sin(x)/x` before committing to anything. If a tree walk sustains an interactive
frame rate while panning, Option A is fine and the rest is premature.

## Cost of deferring

**The decision is cheap to defer. Foreclosing option C is not.**

What must be decided in M1 is narrower: does anything outside `eval` receive a
raw AST? Keep the tree behind an interface and this stays open. Hand trees around
freely and by M6 the choice is already made.
