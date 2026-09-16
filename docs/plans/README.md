# Feature plans

Living working docs for features in progress — one Markdown file per feature. These let a
multi-session feature keep its context (goal, plan, decisions, progress, open questions)
outside the chat, so a new session can pick up where the last left off.

This is the **mutable** counterpart to [`../decisions/`](../decisions/README.md):

- **`docs/decisions/` (ADRs)** record *why* a decision was made and are **immutable** once
  accepted.
- **`docs/plans/` (here)** are **living scratchpads** for a feature while it's built: the
  goal, the evolving plan, progress, and open questions. They change constantly across
  sessions and move to `archive/` once the feature ships and its durable decisions have moved
  into ADRs / CLAUDE.md.

## Conventions

- One file per feature, kebab-case slug: `docs/plans/<feature>.md`.
- Keep it terse — it's a scratchpad, not prose. Suggested sections: Goal, Plan/scope,
  Decisions log, Progress/status, Open questions, `## Session log` (dated entries).
- `/feature-log` appends new info from a session into the active plan doc.
- When a durable design decision crystallizes, promote it to an ADR via `/sync-docs` (the
  plan doc then links to that ADR).
- Once a feature ships and its durable decisions live in ADRs / CLAUDE.md, move its plan doc
  to `archive/` (kept for history, no longer an active scratchpad).
