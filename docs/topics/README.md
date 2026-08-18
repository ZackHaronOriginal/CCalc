# Topics

The project's **controlled vocabulary** — the fixed set of terms every tracked
item is filed under. [`INDEX.md`](INDEX.md) is the list; one file per topic here
holds its definition.

## Why this is a separate area from the topic folders elsewhere

You will see the word "topic" in two places, and they are not the same thing:

| Where | Shape | What it is |
|---|---|---|
| `docs/topics/expr.md` | a **file** | the *definition* of the topic |
| `docs/adr/expr/` | a **folder** | *items filed under* that topic |

This is the standard separation between a controlled vocabulary and the content
tagged with it. The vocabulary lives in one place and defines the terms; every
other area uses those terms as labels.

**The rule that keeps it unambiguous: this directory contains files, never
folders.** Everywhere else, a directory named after a topic holds items. Here, a
*file* named after a topic defines it. If you ever find yourself creating
`docs/topics/expr/`, something has gone wrong — the items belong in
`adr/expr/`, `bugs/expr/` and so on.

## What a topic file is for

Not a summary of the module — `libs/expr/README.md` already does that, and
duplicating it guarantees the two drift apart.

A topic file answers the question the module README cannot: **"is this thing I am
filing an `expr` item or an `eval` item?"** So each one states its scope, what it
explicitly excludes, and how to tell it apart from its neighbours.

## Rules

- **Use an existing topic.** Inventing `parser` next to `expr` destroys the value
  of a shared vocabulary — that is the entire point of a controlled list.
- **A new topic needs a new module, or a genuinely new cross-cutting concern.**
  Add its file and its `INDEX.md` row in the same commit.
- **One topic per item.** If something spans two, file it under the one that
  would have to change and mention the other in the item's `Related` field.
  Cross-references are cheap; duplicated items are not.
- **Do not list items in a topic file.** Link to the topic's folder in each area
  instead. Item lists here would duplicate the area indexes and go stale — and
  the area index is always the authority.
