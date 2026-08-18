# R-003. Where do plugins belong on disk on Linux?

**Topic:** `plugins` · **Status:** complete
**Started:** 2026-08-18 · **Updated:** 2026-08-18
**Related:** ADR 0006, F-012

## The question

Which directories should `plugin-host` search, in what order, and what does Linux
convention actually require?

## The answer

Search three locations, first match winning:

```
1. --plugin-dir <path>              explicit flag, for development
2. $XDG_DATA_HOME/ccalc/plugins/    user's own, no root needed
                                    (default ~/.local/share/ccalc/plugins/)
3. /usr/lib/ccalc/plugins/1/        system-wide, ABI-versioned
```

**Never search the current working directory.** Someone running CCalc inside a
downloads folder would execute whatever happens to be sitting there.

## What this means for CCalc

- Include the **ABI version as a path component** (`/1/`) from the very first
  release. When ABI 2 arrives, both generations coexist in separate directories
  instead of conflicting. Adding that level later is a breaking change; adding it
  now costs nothing.
- Ship plugin headers in a separate `ccalc-dev` package so third parties can
  build against an installed SDK.
- The user-level path needs no root, which is what makes casual plugin
  installation practical.

## Detail

There is a wrinkle worth stating plainly. The XDG Base Directory Specification
covers *data* files: `$XDG_DATA_HOME` (default `~/.local/share`) and
`$XDG_DATA_DIRS` (default `/usr/local/share:/usr/share`).

Plugins are **executable code, not data**, and Linux convention puts executable
code under a library directory — `/usr/lib/<app>/` — not `/usr/share/`. So the
system path deliberately does not follow XDG.

XDG has no user-level equivalent of `lib`, so for the per-user path
`$XDG_DATA_HOME/ccalc/plugins/` is the pragmatic choice and what comparable
applications do. It is a small inconsistency, chosen knowingly.

## Rejected

**`$XDG_DATA_DIRS` for the system path.** Consistent with XDG, but it would put
executable code in `/usr/share/`, which packagers and distro policies object to.

**A single hardcoded directory.** Simpler, but requires root to install any
plugin, which kills casual use.

**Searching the working directory.** Convenient during development — and the
reason the explicit `--plugin-dir` flag exists instead. As a default it is a
straightforward code-execution hazard.

## Confidence and gaps

**Solid** on XDG specifics — taken from the specification itself.

**Convention, not specification,** for `/usr/lib/<app>/plugins/`. The spec search
turned up no formal standard for versioned plugin directories; this reflects
common practice across applications rather than a documented rule.

**Unverified.** Not yet checked against Debian and Fedora packaging policy. Worth
confirming before the first packaged release — a distro policy violation is
found late and is annoying to fix.
