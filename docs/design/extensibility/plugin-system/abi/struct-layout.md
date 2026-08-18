# ABI struct layout

**Topic:** extensibility / plugin-system / abi · **Status:** draft
**Related:** ADR 0006, R-001, Q-006, F-015

The concrete C structures crossing the plugin boundary. **Draft** — nothing here
is frozen until ABI v1 is published, and after that none of it can change.

## Entry point

```c
#define CCALC_ABI_VERSION 1

typedef struct {
    uint32_t    abi_version;    /* MUST be first — read before anything else */
    const char* name;
    const char* version;
    int  (*register_all)(CCalcRegistry*);
    void (*shutdown)(void);
} CCalcPluginInfo;

extern "C" const CCalcPluginInfo* ccalc_plugin_entry(void);
```

`abi_version` is first because it is the only field whose offset can never move.
The host reads it before trusting the rest of the struct — which is the whole
point, since a v2 struct may reorder everything after it.

## Why every field is a pointer or a fixed-width integer

No `size_t` (varies by platform), no `bool` (size not fixed in C), no enums
(underlying type is implementation-defined). Fixed-width integers and pointers
have predictable layout everywhere we care about.

## Ownership

Strings the plugin returns are owned by the plugin and must stay valid until
`shutdown`. The host never frees them. Anything the host passes in is valid only
for the duration of the call — a plugin keeping it must copy.

This asymmetry is deliberate and simple enough to state in one sentence in the
author guide, which matters more than elegance. Allocator mismatches corrupt the
heap and crash somewhere unrelated much later.

## The unresolved part

Argument passing is not settled. Q-006 asks whether v1 carries arrays; the answer
changes this file substantially. Three candidate shapes are sketched in that
question.

**Do not freeze this file until Q-006 is answered and `plugins/stats` has been
written against a draft of it.**
