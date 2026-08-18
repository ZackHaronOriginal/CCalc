# plugin-host

**Layer L3** · target `ccalc_plugin_host` · namespace `ccalc::plugin` · `<ccalc/plugin_host/…>`

Finds plugin files on disk, loads them, checks they are compatible, and registers
what they provide.

This is the **host** side. The contract plugins compile against is
`libs/plugin-abi/`, at layer 0 — see its README before touching anything here.

## What goes here

- `Loader` — `dlopen`, find the `ccalc_plugin_entry` symbol with `dlsym`,
  `dlclose` on shutdown
- `Discovery` — search the plugin directories in order
- `VersionCheck` — read `abi_version` and refuse anything that does not match
- `Registration` — take what the plugin offers and put it in the registry owned
  by `functions`
- `PluginInfo` — what is loaded, from where, at what version, for a `:plugins`
  command

## Where plugins are searched for, in order

```
1. --plugin-dir <path>              explicit flag, for development
2. $XDG_DATA_HOME/ccalc/plugins/    user's own, no root needed
                                    (default ~/.local/share/ccalc/plugins/)
3. /usr/lib/ccalc/plugins/1/        system-wide; note the ABI version
```

The trailing `1` is the ABI version. It is there from the first release so that
when ABI 2 arrives, both generations coexist in separate directories instead of
conflicting. Adding that path level later would be a breaking change.

**Never load from the current working directory.** Someone running CCalc inside
a downloads folder would execute whatever happens to be sitting there.

## Rules

- **Check the version before touching anything else.** `abi_version` is the first
  field of the struct precisely so it can be read before the rest is trusted.
- **A bad plugin must never take down CCalc.** Failure to load, a missing symbol,
  a wrong version — all produce a clear message and a program that keeps running
  without that plugin.
- **Unload in reverse order** at shutdown, and call each plugin's `shutdown`
  first. Unloading a library whose data is still referenced crashes at exit,
  which looks alarming and is hard to diagnose.
- **Never free memory a plugin allocated**, and never let a plugin free ours.
  Different allocators corrupt the heap, and heap corruption crashes somewhere
  else entirely, long after the real mistake.

## Be honest about trust

A native plugin is ordinary machine code running inside our process. It can read
any file the user can read and do anything the user can do. There is no sandbox,
and building one would be a larger project than the calculator itself.

This is the same deal Vim, GIMP and native browser extensions offer, and it is
fine — as long as we say so plainly in `docs/PLUGINS.md`, load only from the
directories above, and never auto-install anything.

## When to build this

Last. The registry in `functions` comes first, then `plugin-abi` sketched
alongside the core modules, and this only once the value and function types have
stopped moving. The loading code is small; the commitment it creates is not.
