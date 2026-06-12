---
description: Add tests for behavior not yet covered, then run the project's test suite
argument-hint: "[area to focus on] (optional)"
---

Extend the project's test suite to cover behavior that currently lacks tests. This command
is stack-agnostic — read `CLAUDE.md` for the concrete test runner, test directory, and any
test-isolation rules (look for a `## Run the tests` / `## Test` section).

Steps:

1. **Map coverage:** read the existing tests (the project's test directory and any shared
   fixtures/config) and the code under test to see what is and isn't exercised.
2. **Identify real gaps:** new or changed public behavior, storage/IO logic, edge cases,
   error paths. If `$ARGUMENTS` names an area, focus there.
3. **Add tests following existing conventions:**
   - Match the style of existing tests (naming, structure, assertions).
   - Respect the project's **test isolation** rule from `CLAUDE.md` (e.g. an in-memory or
     throwaway database / sandbox — never touch real/production data or services).
4. **Run the whole suite** using the project's documented test command.
5. **Make the tests pass.** If a test reveals a real bug, report it — do NOT weaken the
   assertion to make it green.
6. Add only meaningful tests; don't pad with trivial or duplicate cases.
