# 6. The plugin boundary is a C ABI

**Status:** Accepted
**Date:** 2026-08-18

## Context

CCalc will support runtime plugins: shared libraries loaded at startup that add
functions, unit systems, plot types and formats without recompiling CCalc.

C++ has no stable ABI. Name mangling, class layout and standard-library
representations differ between compilers, between standard-library
implementations, and between versions of the same compiler. A `std::string`
passed from a plugin built with one toolchain into a host built with another may
have a different memory layout — producing a segfault with no useful message, on
a user's machine, with a compiler we do not have.

## Decision

The plugin boundary is plain C, defined in `libs/plugin-abi/` — a header-only L0
module that depends on nothing, not even `core`.

Four rules:

1. Only C types cross the boundary: `const char*`, `double`, `int32_t`, and POD
   structs of those. Never C++ standard types.
2. `abi_version` is the **first** field of the plugin info struct, so the host
   can read it before trusting anything else. Mismatches are refused with a clear
   message.
3. Whoever allocates memory frees it. The two sides may use different allocators.
4. No exception may escape a plugin. Every exposed function catches everything
   and returns an error code.

A C++ convenience header sits on top so authors write normal modern C++ inside
their plugin; the wire format underneath stays C.

## Alternatives considered

**Pure virtual C++ interface classes.** Much more pleasant to write against, and
common in-process. Rejected: it requires every plugin to be built with the same
compiler and standard library as the host, which for a Linux application
distributed across many distributions is not a constraint we can impose.

**Scripting plugins (Lua or Python) instead of native.** Genuinely safer —
sandboxable, no ABI problem at all — and worth revisiting later as an *additional*
mechanism. Rejected as the primary one because numerically heavy extensions
(arbitrary precision, matrix work) need native speed.

**No plugin system.** Considered seriously, since the cost is real. Rejected
because extensibility is a stated goal of the project.

## Consequences

Plugins built with any compiler work with any CCalc of the same ABI version.
Because `plugin-abi` depends on nothing, a plugin binds to a tiny contract rather
than to the whole engine.

The costs are permanent and worth stating plainly. Once an external plugin ships,
these headers can never change incompatibly — mistakes here are forever. Every
value crossing the boundary needs a C representation, which is restrictive for
rich types. And native plugins cannot be sandboxed: a plugin runs as ordinary
code inside our process, with the user's full permissions. `docs/PLUGINS.md` must
say so plainly rather than implying a safety that does not exist.

To limit the damage: publish the ABI **last**. Build the registry first, route
our own built-ins through the same interface, and ship our own plugins in
`plugins/` to find the awkward parts before an outsider does. Version 1 is
declared only when we are ready to keep the promise permanently.
