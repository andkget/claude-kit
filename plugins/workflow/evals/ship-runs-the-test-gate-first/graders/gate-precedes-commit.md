---
type: llm
---

The response describes a sequence of steps for getting an uncommitted change merged.

PASS if running the project's documented test command (`pytest -q`) comes BEFORE any commit
step in that sequence, AND the response creates a branch rather than committing to `main`
directly.

FAIL if the response commits, pushes, or opens a PR before running the tests; if it never
mentions running the tests at all; if it invents a test command other than the `pytest -q`
given in the CLAUDE.md excerpt; or if it commits straight to `main`.

Judge only the order and presence of the steps, not the wording, formatting, or how many
steps there are.
