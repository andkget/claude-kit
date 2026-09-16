# AGENTS.md

> **Claude Code is the primary tool on this project and reads `CLAUDE.md`** (it does not
> read this file at all). This file orients **Codex**, used here as a
> token-backup and for code-review / bug-fixes. It is generic and the same in every
> project — the project's details live elsewhere.

## Read these first

The project's specifics — what it is, the stack, **Setup / Run / Run the tests**,
architecture, and protected / never-commit files — live in **`CLAUDE.md`** and the ADRs in
**`docs/decisions/`**. Read them; they are the source of truth.

**Ignore the Claude-Code-only parts of `CLAUDE.md`** — anything about slash commands
(`/ship`, `/write-tests`, `/sync-docs`, …), skills, or a file-based memory system. You
can't run those. Everything else — the Setup/Run/Test commands, architecture, and
conventions — applies to you.

## Your role on this project

Claude Code drives day-to-day building. Your strengths here:

1. **Code review** of the current branch/diff — prefer a **read-only sandbox** so you
   review without editing. A second model catches blind spots the authoring model missed.
2. **Localized bug-fixes** where the bug is already understood — keep the diff narrow.
3. **Token-backup general coding** when the Claude Code window is spent.

Follow the project's **git-by-risk** policy (in `CLAUDE.md`): code changes → branch + PR,
green test gate, squash-merge; docs-only/trivial → straight to the base branch. Never
force-push the base branch; stage files by name; never commit the protected/never-commit
files listed in `CLAUDE.md`.

## Hand-off between tools

The active **`docs/plans/<feature>.md`** is the cross-session, cross-tool hand-off doc:
**read it before starting, and update it (current state + next steps) before you stop.**
**Git is the boundary** — commit (and push, or use `git worktree` for parallel work)
before switching tools, so the next tool picks up clean state.
