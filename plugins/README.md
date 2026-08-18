# plugins/

First-party plugins — extensions we write ourselves, built as separate `.so`
files exactly the way a third party's plugin would be.

## Why these exist

Not because the code could not live in `libs/`. Because **the plugin system has
to be used by someone, and it should be us first.**

If our own plugins go through the real interface, then every build exercises the
plugin path and it cannot quietly rot. We discover the awkward parts of the ABI
before an outsider does — while it is still free to change. And `plugins/example`
gives third-party authors something real to copy instead of a paragraph of
documentation.

## Rules for everything in here

- **Compiles against `libs/plugin-abi/` and nothing else.** Never against `core`,
  `eval`, or any other module. If a plugin needs something the ABI does not
  expose, that is a finding about the ABI, not a reason to reach past it.
- Each subdirectory is fully self-contained, with its own `CMakeLists.txt`,
  sources, tests and README — a miniature project.
- Each builds to a shared library, not a static one.
- No C++ types cross the boundary. Plain C at the doorway, normal C++ inside.
- No exception may escape. Every exposed function catches everything and returns
  an error code.

## What does NOT go here

- Core functionality. If CCalc is not a usable calculator without it, it belongs
  in `libs/`. Plugins are for things a reasonable user might never want.
- Third-party plugins. Those live in their own repositories and are not part of
  this one.

## Good candidates for future plugins

Financial functions · number theory · symbolic algebra (heavy dependency, best
kept optional) · currency conversion (needs network access) · CSV and LaTeX
import/export · polar and 3D plot types.

The pattern: **things a reasonable person might never want, or that drag in a
dependency most users should not have to install.**
