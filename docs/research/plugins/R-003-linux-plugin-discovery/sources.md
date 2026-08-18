# Sources — R-003

| Source | Used for |
|---|---|
| [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir/latest/) | `$XDG_DATA_HOME` and `$XDG_DATA_DIRS` semantics and defaults |
| [XDG Base Directory — ArchWiki](https://wiki.archlinux.org/title/XDG_Base_Directory) | How applications apply the spec in practice |

## Confidence notes

The XDG values are taken from the specification and are reliable.

**The `/usr/lib/<app>/plugins/<abi>/` convention is not from any specification.**
The search found no formal standard for versioned plugin directories; it reflects
observed practice. Distro packaging policy (Debian Policy, Fedora Packaging
Guidelines) has **not** been checked and should be before the first release.
