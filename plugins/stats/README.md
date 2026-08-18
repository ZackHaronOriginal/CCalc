# stats — statistics plugin

A real, useful plugin: statistical functions over lists of numbers.

Where `example` exists to be read, this one exists to be **used** — and to prove
the plugin system can carry real functionality, not just a toy.

## Planned functions

`mean` · `median` · `mode` · `stddev` (sample and population) · `variance` ·
`quantile` · `correlation` · `linreg` (linear regression)

## Why statistics, and why as a plugin

It is the honest test case. Statistics functions take a *list* of values rather
than one number, which means passing an array across the C boundary and getting
back a structured result. If the ABI can handle that comfortably, it can handle
most things people will want to write.

If it turns out to be painful, that is exactly the kind of thing we need to learn
before the ABI is published — which is the whole reason to write our own plugins
first.

## Note on the algorithms

The mathematics should live in `libs/numeric/`, tested there with plain numbers.
This plugin is a thin layer that exposes it by name through the plugin interface.

That may look like an odd split, but it is deliberate: it keeps the hard,
correctness-critical code where it can be tested easily, and keeps the plugin
focused on demonstrating the *interface*. It also means that if we later decide
statistics should be built in rather than a plugin, the move is trivial.
