# ABI versioning policy

**Topic:** extensibility / plugin-system · **Status:** draft
**Related:** ADR 0006, Q-006, F-015

Applies to the ABI, the host and the discovery paths together, which is why it
sits here rather than in `abi/`.

## The rule

**`CCALC_ABI_VERSION` is a single integer that increments only on an
incompatible change.** No minor versions, no feature flags, no negotiation. A
plugin either matches or it is refused.

Simplicity is the point. Every mechanism for partial compatibility is a mechanism
that can be got wrong, and getting it wrong means a crash in someone else's
process.

## What counts as incompatible

| Change | Breaks? |
|---|---|
| Adding a field to the **end** of a struct | yes — the plugin allocates the old size |
| Reordering or removing fields | yes |
| Changing a function-pointer signature | yes |
| Changing what an existing error code means | yes |
| Adding a **new** error code | no, if plugins treat unknown codes as generic failure |
| Adding a new registration function alongside existing ones | no |

The middle two rows are why `abi_version` must be the first field: it is the only
one whose offset survives every change above.

## Supporting two versions

When v2 arrives, the host accepts both for at least two releases. That means:

- Two entry-point symbols, or one that branches on the version it read
- Two registration paths in `plugin-host`
- **Two install directories** — `/usr/lib/ccalc/plugins/1/` and `.../2/` — which
  is why the version is in the path from the very first release. Adding that
  level later would itself be a breaking change.
- `plugins/example/` built against both, so the guide stays truthful

That is the real cost of a v2, and it is why v1 should not be published early.

## Refusal behaviour

A version mismatch is **not** an error the user caused, so the message says what
to do:

```
plugin 'stats' targets ABI 2, this build supports ABI 1 — not loaded
  install a build of 'stats' for CCalc 0.x, or upgrade CCalc
```

CCalc continues running without it. A plugin failing to load must never take down
the calculator.

## When v1 gets declared

Not at M7. **Only once `plugins/stats` has been written against the draft and
Q-006 is answered.** Until we publish, changing any of this is free; after, none
of it is.
