# Bug list

Every known defect, one line each. **If it is not listed here, it is not tracked.**

Next free ID: **B-001**. IDs are never reused, including for `invalid` reports.

## Open

*None.* There is no code yet — `apps/cli/main.cpp` is still a Hello World stub.
The first entries arrive with M1.

Row format, for when they do:

| ID | Topic path | Sev | Summary | Status | Detail |
|---|---|---|---|---|---|
| B-001 | expression-engine / lexical-handling | wrong | `1.2.3` lexes as two numbers instead of erroring | confirmed | `expression-engine/lexical-handling/B-001-malformed-number.md` |

*(example row showing the format — replace when the first real bug is filed)*

## Fixed

*None yet.*

## Won't fix / invalid

*None yet.*

---

## Reminders

- **Every fix ships with a regression test.** No exceptions.
- **Never skip or disable a test to get a green build.** That turns a visible
  problem into an invisible one, which is strictly worse.
- `wrong` severity outranks almost everything. A crash is obvious to the user; a
  confidently incorrect number is not.
