# packaging/

Everything needed to turn a built CCalc into something a user can install.

## What goes here

- `debian/` — control files for `.deb` packages (Debian, Ubuntu, Mint)
- `rpm/` — spec file for `.rpm` packages (Fedora, openSUSE)
- `arch/` — `PKGBUILD` for Arch and derivatives
- `flatpak/` — the Flatpak manifest
- `appimage/` — AppImage recipe, for a single-file portable build
- `snap/` — snapcraft config, if we choose to support it

## Install layout we target

| What | Where |
|---|---|
| Executables | `/usr/bin/ccalc` |
| Plugins | `/usr/lib/ccalc/plugins/1/` |
| Plugin SDK headers | `/usr/include/ccalc/plugin/` |
| Icons, desktop entry | `/usr/share/…` |

The `1` in the plugin path is the ABI version. It is there from the very first
release so that when ABI 2 arrives, version 1 and version 2 plugins sit in
separate directories and coexist instead of conflicting. Adding that directory
level later is a breaking change; adding it now costs nothing.

## Rules

- Packaging must never require patching the source. If a distro needs something
  different, that is a build option, not a patch.
- Ship the plugin headers in a separate `ccalc-dev` package, so plugin authors
  can build against a stable installed SDK.
