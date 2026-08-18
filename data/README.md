# data/

Files the *installed program* needs at runtime. Everything here gets copied to
the user's system when CCalc is installed.

## What goes here

- `icons/` — application icons at the sizes freedesktop expects
  (16, 24, 32, 48, 64, 128, 256 px, plus a scalable SVG)
- `ccalc.desktop` — the launcher entry, so CCalc appears in application menus
- `ccalc.metainfo.xml` — AppStream metadata, so it appears in GNOME Software,
  KDE Discover and Flathub with a description and screenshots
- `translations/` — `.po` files for other languages
- `themes/` — colour schemes for plots and the GUI

## What does NOT go here

- Test fixtures. Those go in `tests/fixtures/`.
- Documentation. That goes in `docs/`.
- Anything only used while building. That goes in `tools/`.

## The test for whether something belongs here

Ask: *would this file need to exist on a user's machine for CCalc to work
correctly?* Yes means `data/`. No means somewhere else.

## Note on Linux packaging

These are data files, so they install under `/usr/share/` — icons to
`/usr/share/icons/hicolor/`, the desktop entry to `/usr/share/applications/`.
Plugins are the exception: they are executable code and install under
`/usr/lib/ccalc/plugins/`, not here.
