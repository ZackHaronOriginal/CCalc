# Open questions

Things we have not decided. One file, organised by topic — no folder, because a
question that needs its own folder has become research.

Next free ID: **Q-008**. IDs are never reused.

## How a question ends

A question is *closed*, never deleted. It ends one of four ways:

| Ending | What happens |
|---|---|
| **Decided** | becomes an ADR — move it to Answered with the link |
| **Needs investigation** | becomes a research entry — link it, keep the question open until the research concludes |
| **Answered trivially** | record the answer inline and move it to Answered |
| **No longer relevant** | move it to Answered with the reason it stopped mattering |

Keeping closed questions matters. "Why don't we use GMP?" gets asked repeatedly,
and a one-line answer with a link is much cheaper than rediscovering the reason.

---

## Open

### `gui`

**Q-001 · Which UI toolkit for the desktop frontend — Qt or GTK?**
Affects licensing, packaging size, and how good the graph canvas can be. Qt has
the stronger canvas and better cross-platform story; GTK integrates more
naturally on GNOME and has simpler licensing.
*Not urgent* — ADR 0005 keeps `libs/plot` toolkit-free, so this stays cheap and
reversible until M8. Needs research before deciding.
→ blocks F-017

### `build`

**Q-002 · Catch2 or doctest for the test framework?**
Catch2 is more widely known; doctest compiles substantially faster, which matters
across a dozen modules. Test bodies look nearly identical either way, so
switching later is cheap — which argues for just picking one now.
*Decide at M0.* → blocks the whole test setup

### `numeric`

**Q-003 · Write our own arbitrary-precision arithmetic, or depend on GMP/MPFR?**
GMP is fast and correct but is LGPL, which affects static linking and packaging.
Our own would be slower and take real effort to get right, but keeps
distribution simple. Needs research covering licensing, binary size, and
packaging implications on each target distro.
→ blocks F-010

### `core`

**Q-004 · How does `Value` represent a united quantity?**
Does a unit live inside `Value` as another variant arm, or does `Value` carry an
optional dimension alongside its number? This decides whether every arithmetic
operation must consider units, or only the ones that opt in.
Affects `core`, `eval` and `units` together, so it should be settled before M5 —
changing it later touches every operation. Likely an ADR.
→ blocks F-012, F-019

### `eval`

**Q-005 · Is angle mode (degrees vs radians) global state, or part of a call?**
Global is what physical calculators do and what users expect. Global mutable
state is also the classic source of "why is my answer wrong" confusion, and it
makes plotting trigonometric functions order-dependent.
→ affects F-005, F-013

### `plugins`

**Q-006 · Must ABI version 1 be able to pass arrays?**
Statistics functions take a list of values, so the C boundary needs an array
representation with a length. If v1 ships without it, every list-taking plugin
waits for v2 — and v2 means supporting two ABIs.
**This must be settled before ABI v1 is published, not after.** F-016 exists
partly to answer it.
→ blocks F-015, F-016

### `session`

**Q-007 · What format do saved workspaces use — JSON, TOML, or custom?**
Whatever we pick, it needs a version number as the first field and a loader that
refuses unknown versions rather than guessing. Users will have saved work they
care about; silently misreading it is much worse than declining to open it.
→ blocks F-018

---

## Answered

*None yet.* Answered questions move here with their resolution and a link to the
ADR, research entry, or one-line answer that settled them.

Format:

> **Q-000 · Should modules nest?** — No. ADR 0001: nesting grows without limit
> and nobody remembers whether statistics lives under `math/stats/` or
> `numeric/statistics/`. `libs/` is flat.
