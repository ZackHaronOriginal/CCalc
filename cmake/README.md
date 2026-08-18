# cmake/

Reusable CMake helper modules. Code that build files *include*, not build files
themselves.

## What goes here

- `CCalcModule.cmake` — a helper function that defines a `libs/` module the same
  way every time (target name, include directories, warning flags, test wiring)
- `CompilerWarnings.cmake` — the shared warning set, in one place
- `FindXxx.cmake` — locators for dependencies CMake does not find on its own
- `toolchains/` — cross-compilation toolchain files, if we ever need them

## What does NOT go here

- The root `CMakeLists.txt`. That stays at the project root.
- A module's own `CMakeLists.txt`. That lives in the module folder.

## Why this folder exists

With a dozen modules, the same twenty lines of build boilerplate would be copied
a dozen times. One `ccalc_add_module()` helper here means a module's build file
is three lines, and changing the warning flags for the whole project is a
one-line edit instead of a twelve-file sweep.
