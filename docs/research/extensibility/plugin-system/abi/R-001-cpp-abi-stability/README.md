# R-001. Why does C++ have no stable ABI, and what does that force on our plugin design?

**Topic:** `plugins` · **Status:** complete
**Started:** 2026-08-18 · **Updated:** 2026-08-18
**Related:** ADR 0006, F-012, Q-006

## The question

CCalc will load plugins compiled by other people, with compilers we do not
control. What can safely cross that boundary?

## The answer

**Almost nothing from C++ can cross it.** The language has no standard ABI: name
mangling, class memory layout, and standard-library representations all vary
between compilers, between standard-library implementations, and between versions
of the same compiler.

`std::string` is the canonical trap. Its layout is not specified by the standard,
so libstdc++ and libc++ lay it out differently. Passing one across the boundary
is handing someone a sealed box while they read the wrong instructions for
opening it — a segfault with no useful message, on a user's machine, with a
compiler we do not have.

**The boundary must therefore be plain C.**

## What this means for CCalc

1. `libs/plugin-abi/` is C, not C++. `const char*`, `double`, `int32_t`, and POD
   structs of those. Entry points marked `extern "C"` so names are not mangled
   and `dlsym` can find them.
2. A C++ convenience header may sit *on top*, so authors write normal modern C++
   inside their plugin. The wire format underneath stays C.
3. `abi_version` must be the **first** field of the info struct — the one field
   whose position can never move, so the host can read it before trusting
   anything else.
4. **Whoever allocates, frees.** The two sides may use different allocators, and
   cross-allocator frees corrupt the heap — which then crashes somewhere else
   entirely, long after the real mistake.
5. **No exception may escape.** A C++ exception crossing a C boundary is
   undefined behaviour, usually an immediate crash with no information. Every
   exposed function catches everything and returns an error code.

This is recorded as ADR 0006.

## Detail

The ABI comprises name mangling, struct padding and alignment, class layout
including vtable placement, and the representation of standard-library types.
None of it is specified by the C++ standard, which deliberately leaves it to
implementations.

C, by contrast, has a *de facto* stable ABI on each platform — stable enough that
essentially every language can call C. That is why C is the universal boundary
language, not because it is better, but because everyone has already agreed how
its types are laid out.

## Rejected

**Pure virtual C++ interface classes.** Much nicer to write against, and common
for in-process plugins. Rejected: it requires every plugin to be built with the
same compiler *and* standard library as the host. For a Linux app distributed
across many distributions, that is not a constraint we can impose.

**Requiring one blessed compiler version.** Would make C++ interfaces safe.
Rejected as unenforceable — we cannot police what a third party builds with, and
the failure mode is a silent crash rather than a clear error.

**Scripting plugins (Lua, Python) instead of native.** Genuinely safer:
sandboxable, no ABI problem at all. Rejected as the *primary* mechanism because
numerically heavy extensions need native speed — but worth revisiting later as an
additional path, since it would let untrusted plugins be safe.

## Confidence and gaps

**Solid.** The no-stable-ABI property is well documented and long established;
the C-boundary approach is what essentially every plugin system in C++ does.

**Untested by us.** We have not yet built a plugin and loaded it across two
compilers. That test should exist before ABI version 1 is published — it is the
only way to know the design actually holds.

**Open.** Whether arrays can be passed comfortably in ABI v1 (needed for
statistics functions taking a list) is unresolved — tracked as Q-006, and the
reason `plugins/stats/` exists as a first-party test case.
