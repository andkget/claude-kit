---
type: llm
---

The user describes a convention they call broadly applicable, not specific to their project,
and asks where it should live.

PASS if the response routes it to the shared kit — the `claude-kit` repository, the
`workflow` plugin, or the `project-conventions` skill — rather than only into this project's
own `CLAUDE.md` or docs. A response that puts it in the kit AND notes the project can also
reference it is a PASS.

FAIL if the response only edits this project's `CLAUDE.md`, `docs/decisions/`, or
`docs/plans/` and never mentions contributing it back to the shared kit; or if it declines to
place the rule anywhere.

A PASS is strengthened, but not required, by mentioning that the plugin's version must be
bumped for the change to reach installed projects.
