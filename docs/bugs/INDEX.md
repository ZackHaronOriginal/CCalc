# Bug list

Every known defect, one line each. **If it is not listed here, it is not tracked.**

Next free ID: **B-001**. IDs are never reused, including for `invalid` reports.

## Open

*None.* There is no code yet — `apps/cli/main.cpp` is still a Hello World stub.

The first entries will arrive with M1. Format:

| ID | Topic | Sev | Summary | Status | Detail |
|---|---|---|---|---|---|
| B-001 | expr | wrong | `2 +` parses as `2` instead of erroring on the trailing operator | confirmed | `expr/B-001-trailing-operator.md` |

*(example row showing the format — replace it when the first real bug is filed)*

## Fixed

*None yet.*

## Won't fix / invalid

*None yet.*

---

## Reminders

- **Every fix ships with a regression test.** No exceptions.
- **Never skip or disable a test to get a green build.** That turns a visible bug
  into an invisible one, which is strictly worse.
- `wrong` severity outranks almost everything. A crash is obvious to the user; a
  confidently incorrect number is not, and that is the failure a calculator can
  least afford.
