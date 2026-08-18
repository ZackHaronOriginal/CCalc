# Plugin system overview

**Topic:** extensibility / plugin-system · **Status:** draft
**Related:** ADR 0006, R-001, R-003, F-015, Q-006

How the pieces fit together. The subtopic folders here cover each piece in
detail; this note is the map.

## Three pieces, two layers

```
   plugin author's .so                CCalc process
   ┌──────────────────┐        ┌──────────────────────────────┐
   │  their code      │        │  plugin-host    (L3)         │
   │  ┌────────────┐  │        │  finds, loads, version-checks│
   │  │ normal C++ │  │        │             │                │
   │  └──────┬─────┘  │        │             ▼ registers into  │
   │  ┌──────▼─────┐  │  dlopen│      registry   (L2)         │
   │  │  C doorway ├──┼───────▶│      name → callable         │
   │  └────────────┘  │        │             ▲                │
   └────────┬─────────┘        │             │ asks           │
            │ compiles against │          eval  (L2)          │
            ▼                  │                              │
      plugin-abi  (L0)  ◀──────┼──────────────────────────────┘
      header-only, no deps     └──────────────────────────────
```

| Piece | Where | Job |
|---|---|---|
| `plugin-abi` | `libs/plugin-abi/`, L0 | the C contract. Depends on nothing. |
| `plugin-host` | `libs/plugin-host/`, L3 | discovery, `dlopen`, version check, registration |
| the registry | `libs/functions/`, L2 | the table `eval` reads, and the host writes |

## The one idea worth internalising

**`eval` never learns that plugins exist.** It asks the registry for a name and
calls what comes back — built-in or loaded three seconds ago, it cannot tell.

That is why the plugin system is purely *additive*: nothing in L0, L1 or L2
changes when it arrives. It only works because `plugin-abi` sits at the bottom
depending on nothing, and `plugin-host` sits at the top depending on everything.
Collapsing those into one module would break it.

## The subtopics

| Folder | Covers |
|---|---|
| `abi/` | the C structs, argument passing, error codes |
| `host/` | loading sequence, symbol resolution, unload order |
| `discovery/` | where plugins are searched for, and in what order |

## What is settled and what is not

**Settled** (ADR 0006): plain C at the boundary · `abi_version` first · whoever
allocates frees · no exception escapes · `extern "C"` entry points.

**Open:** argument passing (Q-006 — arrays in v1?) · whether plugins can load
without a restart · whether a scripting path is added later alongside the native
one.

## Order of work

The loader is the small part. The commitment is the large part — see
`versioning-policy.md`.
