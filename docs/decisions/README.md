# Architecture Decision Records

This directory records the significant design decisions for the project. Each ADR captures
the context, the options considered, the decision, and the reasoning, so the *why* behind
the codebase stays legible over time.

Each file is immutable once accepted: to change a decision, add a new ADR that supersedes the
old one (note the supersession in both files) rather than editing history.

## Index

| # | Decision | Status |
|---|----------|--------|
| [0000](0000-adr-template.md) | ADR template (copy me) | Template |
| [0001](0001-conventions-ship-as-a-plugin-not-copies.md) | Conventions ship as an installable plugin, not as copies per repo | Accepted |
| [0002](0002-claude-code-first-codex-as-second-tool.md) | Claude Code is primary; Codex is oriented by a generic `AGENTS.md` | Accepted |
| [0003](0003-quality-gates-cover-the-agents-output.md) | Quality gates cover the agent's output, not the human's edits | Accepted |
| [0004](0004-a-kit-change-ships-only-with-a-version-bump.md) | A kit change ships only with a version bump | Accepted |

## Adding a new ADR

1. Copy `0000-adr-template.md` (structure: context → options considered → decision → why →
   consequences).
2. Use the next sequential number and a short kebab-case slug.
3. Add a row to the index table above.
