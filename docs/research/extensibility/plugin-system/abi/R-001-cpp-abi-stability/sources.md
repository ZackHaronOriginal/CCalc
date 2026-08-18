# Sources — R-001

| Source | Used for |
|---|---|
| [Shared Libraries — Binary Components](https://caiorss.github.io/C-Cpp-Notes/DLL-Binary-Components-SharedLibraries.html) | C-style types at the boundary; `extern "C"` entry points; the `dlopen`/`dlsym` pattern |
| [Stability of the C++ ABI: Evolution of a Programming Language (Oracle)](https://www.oracle.com/technical-resources/articles/it-infrastructure/stable-cplusplus-abi.html) | What the ABI comprises — name mangling, padding, class layout — and why versions cannot mix |
| [Boost.DLL design rationale](https://github.com/apolukhin/Boost.DLL/blob/9f9471cc3351273d73c2cd7328d37010f5708115/doc/design_rationale.qbk) | Prior art: how an established C++ library handles the same constraints |

## Confidence notes

All three are secondary sources — documentation and design rationale, not
measurements we took. The claims are consistent across them and match long-standing
common practice, so confidence is high, but **nothing here has been verified by
building against two toolchains ourselves.** Do that before publishing ABI v1.
