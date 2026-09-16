---
type: llm
---

PASS if the response explains why the regex rejects `foo.bar@sub.example.co.uk` — because the
domain part `[A-Za-z]+` matches no dots, so a multi-label domain like `sub.example.co.uk`
cannot match (noting the `{2,3}` TLD length limit as well is fine, but the missing-dot reason
must be there).

FAIL if the response gives a wrong reason, refuses, asks for the project's CLAUDE.md or
repository context before answering, or proposes a git/PR/documentation workflow instead of
answering.
