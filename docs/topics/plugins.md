# plugins

**Layers:** L0 and L3 · **Modules:** [`libs/plugin-abi/`](../../libs/plugin-abi/README.md), [`libs/plugin-host/`](../../libs/plugin-host/README.md)

Runtime extensibility: the frozen C contract, the loader, and everything about
authoring plugins.

## Why one topic spans two layers

The system deliberately splits to opposite ends of the stack. `plugin-abi` sits
at L0 and depends on **nothing**, so a plugin binds to a tiny contract rather
than the whole engine. `plugin-host` sits at L3 and depends on nearly everything,
because loading and registering requires the full picture.

They are one topic because they are one concern: change the ABI and the host
changes with it.

## In scope

The C struct definitions, `abi_version`, discovery paths, `dlopen`/`dlsym`,
version refusal, registration, `docs/PLUGINS.md`, and `plugins/example/`.

## Not in scope

The registry table itself (`functions`). What individual first-party plugins
compute — a statistics plugin's mathematics is `numeric`.

## The permanent constraint

**Once an external plugin ships, `plugin-abi` can never change incompatibly
again.** Mistakes here are forever, in a way mistakes elsewhere are not. Anything
filed under this topic that touches the contract deserves extra scrutiny.

## Decisions in force

ADR 0006 — plain C boundary, `abi_version` first, allocator symmetry, no escaping
exceptions.

## Research

R-001 (why C++ has no stable ABI) · R-003 (Linux plugin discovery paths).

## Open

Q-006 — must ABI v1 pass arrays? **Settle before publication, not after.**
