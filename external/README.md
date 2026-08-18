# external/

Third-party source code copied directly into this repository ("vendored").

## Prefer NOT to use this folder

Most dependencies should be fetched by the build instead, using CMake's
`FetchContent`. That keeps the repository small, makes the version obvious in
one line of build config, and makes upgrading a one-line change.

Vendor a dependency here only when there is a real reason:

- The library is tiny, header-only, and unlikely to ever change
- Upstream is unmaintained or has disappeared
- We had to patch it, and the patch cannot go upstream
- A packaging target requires a fully offline build

## If you do vendor something

Create one subdirectory per dependency, and include a `VERSION.md` recording:

1. The upstream URL
2. The exact version or commit hash copied
3. The license, and a copy of the license file
4. **Why** it was vendored rather than fetched
5. Any local modifications made, in detail

Point 5 matters most. An undocumented local patch inside vendored code is
almost impossible to discover later, and it silently breaks the next upgrade.

## What does NOT go here

- Our own code, ever.
- Plugins. First-party plugins go in `plugins/`; third-party plugins are not
  part of this repository at all.
