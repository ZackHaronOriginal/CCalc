# 4. Keep include/ and src/ separate within each module

**Status:** Accepted
**Date:** 2026-08-18

## Context

Two respected conventions disagree about header placement. The Pitchfork Layout
puts public headers in `include/` and sources with private headers in `src/`.
P1204 "Canonical Project Structure", submitted to the C++ committee, puts headers
directly beside their sources — arguing that editing a class means touching both
files, so splitting them across two trees creates constant jumping, and noting
that teams using the split tend to dump private headers into `include/` anyway.

Both arguments are sound. This is a genuine trade-off, not a right answer.

## Decision

Each module uses separate directories:

```
libs/<name>/include/ccalc/<name>/   public
libs/<name>/src/                    private
```

The build grants other modules access to `include/` only.

## Alternatives considered

**Headers beside sources (P1204).** Rejected, with real reluctance — the
navigation argument is correct and it is a daily cost we now pay. It lost on two
points. First, with ten-plus modules, uncontrolled coupling is our largest
long-term risk, and separate placement is the only option where the *compiler*
enforces the public/private boundary rather than discipline. Second, the plugin
system settles it: third-party authors need one directory of headers to compile
against, and this layout hands us exactly that, installable as a `ccalc-dev`
package.

## Consequences

Another module physically cannot include a private header — the build fails
rather than the coupling growing unnoticed. What a module offers is legible by
reading one directory. Packaging the plugin SDK is a directory copy.

The cost is the daily friction P1204 describes: editing one class means two
directories. This is accepted deliberately.

P1204's warning applies to us and must be resisted: **if private headers start
appearing in `include/`, this decision has quietly stopped working.** A header
belongs in `include/` only if another module genuinely needs it.
