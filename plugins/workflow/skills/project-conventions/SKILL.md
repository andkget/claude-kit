---
name: project-conventions
description: The portable engineering house rules behind the workflow commands — ADR discipline, living plan docs, docs-ship-with-code, git ceremony scaled by risk, test discipline, and the scope of automated quality gates. Load when deciding how to document a decision, structure docs, run the git/PR cycle, write tests, or judge whether a project's gate applies to text the user wrote themselves, or when a project's CLAUDE.md references these conventions.
---

# Project conventions (house rules)

These are stack-agnostic practices the `workflow` plugin's commands assume. A project's
`CLAUDE.md` provides the concrete commands; this skill provides the *why* and the shape.

## Two kinds of docs: immutable decisions vs living plans

- **`docs/decisions/` — ADRs (immutable).** Record *why* a significant, lasting design
  choice was made: a real decision between options with consequences, not routine detail.
  Structure: `Status/Date → Context → Options considered → Decision → Why → Consequences`
  (see `docs/decisions/0000-adr-template.md`). Once accepted, an ADR is **never rewritten** —
  to change a decision, add a new ADR that **supersedes** it and note the supersession in
  both files. Keep a numbered index in `docs/decisions/README.md`.
- **`docs/plans/` — living scratchpads (mutable).** One file per in-progress feature: goal,
  evolving plan, decisions log, progress, open questions, dated session log. Update
  constantly. When a feature ships and its durable decisions have moved into ADRs / CLAUDE.md,
  move the plan doc to `docs/plans/archive/`.

## Docs ship with the code that changes them

Documentation that accompanies a code change is updated in the **same PR** as that change —
never left for "later". Standalone doc-only edits can go straight to the base branch (see
git-by-risk). Use `/sync-docs` to promote durable decisions into ADRs / CLAUDE.md, and
`/feature-log` to keep the active plan doc current.

## Git ceremony scaled by risk

- **Code changes** (anything that can break behavior) → always a branch + PR, test-gated,
  squash-merged. Auto-merge once tests are green and the diff is clean; pause and flag if the
  diff is risky, tests are red/skipped, or it touches something irreversible/shared.
- **Docs-only / trivial changes** → may go straight to the base branch per the project's
  stated policy, no PR ceremony.
- Never force-push the base branch. Push prompts for confirmation by default. Stage files by
  name, never blind `git add -A`; never commit the project's protected/never-commit files.

## Test discipline

- Every change that can break behavior gets a test. A test gate (the project's documented
  test command) must be green before a PR merges (`/ship`).
- Tests run **isolated** from real/production data and services (in-memory or throwaway
  sandbox, per the project's `CLAUDE.md`).
- If a test reveals a real bug, fix the code — never weaken the assertion to make it green.
  Add meaningful tests, not padding.

## Quality gates apply to the agent's output, not the human's edits

Some projects run an automated gate over *generated content* — a prose linter, a style
checklist, a script the agent must re-run until it exits clean. That gate exists to keep the
**agent's** drafts honest. It has no authority over text the human wrote.

- The human pastes their own version of the artifact, or edits the file by hand → **take it
  as-is**. At most fix outright typos and grammar, silently.
- Do **not** run the gate over their text, do not report its complaints back to them, and
  never edit their words to satisfy a metric — no padding to reach a minimum length, no
  restoring content they deliberately cut.
- A targeted rewrite they asked for ("redo the second paragraph") is the agent's text again,
  so the gate applies to it.

A human edit is a decision, not a draft. A gate that starts correcting the author has
inverted who works for whom, and it reads as the tool arguing with its owner.

## The CLAUDE.md contract

So the commands stay stack-agnostic, each project's `CLAUDE.md` declares: a project
description, `## Setup` / `## Run` / `## Run the tests` commands, the protected/never-commit
list, and the base branch + git-by-risk policy. The commands read these instead of
hardcoding a stack. `/scaffold` lays down a `CLAUDE.md` template with these sections.

## Two coding tools: Claude Code primary, Codex for review + backup

These projects are built **Claude-Code-first** — Claude Code is the primary tool and reads
`CLAUDE.md`. A second tool, **Codex**, is kept as a **token-backup** (for when the Claude
Code usage window is spent) and for its strengths: **code-review** (run it in a read-only
sandbox — a different model catches the author model's blind spots) and **localized
bug-fixes**.

- **`AGENTS.md` orients Codex; `CLAUDE.md` stays the single source of truth.** `/scaffold`
  lays down a **generic, project-neutral `AGENTS.md`** (identical in every repo) that frames
  Codex's role and points it at `CLAUDE.md` + `docs/decisions/` for the specifics. There is
  **no parallel project doc to keep in sync** — Codex reads the real `CLAUDE.md` (ignoring
  the Claude-only parts: slash commands, skills, the memory system).
- **`CLAUDE.md` is never modified for Codex's sake.** Claude Code reads `AGENTS.md` only as
  a fallback when no `CLAUDE.md` exists, so adding `AGENTS.md` is invisible to Claude Code —
  zero risk to the primary workflow.
- **Hand-off rides the living plan doc.** The active `docs/plans/<feature>.md` is the
  cross-tool hand-off (read before starting, update before stopping); **git is the
  boundary** — commit before switching tools.

## Where a practice lives: this project vs the shared kit

These conventions and commands come from a shared kit repo — **`andkget/claude-kit`** (a
Claude Code marketplace + the `workflow` plugin). Two homes, decided by one question —
*"would my other projects benefit from this?"*

- **This project only** (its architecture, concrete commands, stack gotchas, decisions about
  *this* system) → the local repo: `CLAUDE.md`, `docs/decisions/`, `docs/plans/`.
- **Reusable across projects** (a new or improved workflow command, a generally-useful
  convention, a better ADR/skeleton template, a fix to a house rule) → contribute it **back
  to `claude-kit`**: edit there, commit, push; then `/plugin marketplace update` in projects
  to pull it. Don't copy a reusable improvement into one project's files where it can't
  travel.

When a session produces something reusable, **say so and offer to land it in `claude-kit`**
rather than letting it ossify in a single project.
