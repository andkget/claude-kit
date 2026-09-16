# 0002 — Claude Code is primary; Codex is oriented by a generic AGENTS.md

- Status: Accepted
- Date: 2026-06-14

## Context

A second coding tool, Codex, was worth keeping around for three specific things: reviewing a
finished branch in a read-only sandbox (a different model catches the authoring model's blind
spots), localized bug-fixes where the bug is already understood, and general work when the
Claude Code usage window is spent.

Codex reads `AGENTS.md`. Claude Code reads `CLAUDE.md`. The naive way to support both is to
write the project's setup, run, test, and architecture details into both files — which
creates two documents describing one project, guaranteed to disagree within weeks.

## Options considered

1. **No `AGENTS.md`** — Codex starts cold in every repo and re-derives the project each time,
   or is fed context by hand. Cheapest to maintain, worst at the moment it is actually needed
   (the Claude window is spent and nobody wants to write a briefing).
2. **A project-specific `AGENTS.md`** mirroring `CLAUDE.md`'s content — Codex is well
   oriented, at the cost of a parallel doc that must be kept in sync in every repo forever.
3. **A generic, project-neutral `AGENTS.md`** that frames Codex's role and points it at
   `CLAUDE.md` + `docs/decisions/` for every specific — identical in every repo, so there is
   nothing to sync.

## Decision

`/scaffold` lays down a **generic `AGENTS.md`, byte-identical in every project**. It states
that Claude Code is primary, names Codex's three jobs, tells Codex to read `CLAUDE.md` and
`docs/decisions/` for all project specifics, and tells it to ignore the Claude-only parts of
`CLAUDE.md` it cannot act on (slash commands, skills, the memory system).

`CLAUDE.md` remains the single source of truth and **is never modified for Codex's sake.**
Hand-off between the two tools rides the active `docs/plans/<feature>.md` — read it before
starting, update it before stopping — and **git is the boundary**: commit before switching,
so the next tool picks up clean state.

## Why

Option 3 keeps one source of truth while still giving the second tool a running start. The
file needs no tailoring, so `/scaffold` can copy it unchanged and it never rots.

Adding it is also free on the primary side: Claude Code reads `CLAUDE.md`, not `AGENTS.md` —
the file is invisible to it entirely, so an `AGENTS.md` in the repo cannot perturb the main
workflow.

## Consequences

- `AGENTS.md` is a kit file, not a project file. It is edited here and re-scaffolded, never
  customized per repo — a repo that tailors it has re-created the sync problem this decision
  avoids.
- Codex's usefulness is bounded by how good the project's `CLAUDE.md` is, which is the same
  thing that bounds Claude Code's. One doc to keep good, not two.
- The plan doc in `docs/plans/` is now load-bearing for cross-tool work, not just
  cross-session work.
- If Claude Code ever starts reading `AGENTS.md` natively, this decision needs revisiting —
  the "invisible, therefore free" argument is what makes the generic file safe.
