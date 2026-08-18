# plugin-abi

**Layer L0** · target `ccalc_plugin_abi` (INTERFACE) · `<ccalc/plugin/…>`

The contract between CCalc and plugins. Header-only, plain C, **depends on
absolutely nothing** — not even `core`.

## Read this before changing anything here

Every file in this directory is a **promise**. The moment someone outside this
project ships a plugin, these headers can never change incompatibly again. A
mistake here is permanent in a way that a mistake anywhere else in the project
is not.

## What goes here

- `abi.h` — the C structs and function-pointer types that cross the boundary,
  and the `CCALC_ABI_VERSION` constant
- `registry.h` — the C interface a plugin uses to register what it provides
- `plugin.hpp` — a small, *optional* C++ convenience wrapper so writing a plugin
  is pleasant. This is a courtesy layer on top; the wire format underneath stays
  pure C.

## The rules, and why each one exists

**1. Plain C types across the boundary only.** `const char*`, `double`,
`int32_t`, and simple structs of those. Never `std::string`, `std::vector`, or
any C++ standard type.

The C++ standard does not specify how a `std::string` is laid out in memory.
GCC's version and Clang's version differ, and two GCC versions can differ.
Passing one across the boundary is handing someone a sealed box while they read
the wrong instructions for opening it — a segfault with no useful message, on a
user's machine, with a compiler you do not have.

**2. `abi_version` is always the first field.** It is the one field whose
position can never move, because the host must read it before it knows how to
interpret anything else. If it does not match, the host unloads the plugin and
prints a clear message. A refused plugin is a minor annoyance; a loaded
incompatible plugin is a crash the user will blame on you.

**3. Whoever allocates memory, frees it.** The plugin and host may use different
allocators, and memory allocated on one side and freed on the other corrupts the
heap — which then crashes somewhere else entirely, long after the real mistake.
If a plugin returns a string, it also exports the function that frees it.

**4. Exceptions must never escape a plugin.** A C++ exception crossing a C
boundary is undefined behaviour, usually an instant crash with no information.
Every exposed function is wrapped in `try { … } catch (...) { return error; }`.
Put that wrapper in `plugin.hpp` so authors get it automatically and cannot
forget.

**5. Entry points are `extern "C"`.** Otherwise the symbol name is mangled
differently by each compiler and `dlsym` will not find it.

## To change the ABI

You do not edit version 1. You add version 2 alongside it and support both for a
release or two. `libs/plugin-host/` decides which versions it accepts.

## Keeping it honest before release

Route CCalc's **own** built-in functions through this interface, even before
anything is loaded from disk. If your own code cannot express something through
the ABI, no plugin will be able to either — and you find that out while it is
still free to change.
