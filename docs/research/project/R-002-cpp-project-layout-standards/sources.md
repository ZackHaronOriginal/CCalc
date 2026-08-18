# Sources — R-002

| Source | Used for |
|---|---|
| [The Pitchfork Layout (PFL)](https://joholl.github.io/pitchfork-website/) | `libs/` rules, the no-nesting constraint, the separated-vs-merged header discussion, the "extremely heavy tool" warning |
| [vector-of-bool/pitchfork](https://github.com/vector-of-bool/pitchfork) | The specification's home and full directory list |
| [P1204R0 — Canonical Project Structure](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1204r0.html) | The case for headers beside sources; the include-prefix rule; naming and file-extension scheme |
| [Godot Engine](https://github.com/godotengine/godot) | A real large modular C++ application's top-level layout |
| [Separating UI from business logic (ICS)](https://www.ics.com/blog/heres-why-you-should-separate-ui-business-logic-your-application) | The reasoning behind the `apps/`-holds-no-logic rule |

## Confidence notes

PFL and P1204 were read directly and their rules quoted, not recalled — high
confidence.

Godot's layout was read from its repository listing only. We did **not** examine
how its modules actually depend on each other, so it supports the claim about
*directory naming* but not any claim about its internal coupling.

**Only one large application was examined.** Blender, Qt and LLVM were not looked
at. A wider survey would strengthen the conclusion, and none was done.
