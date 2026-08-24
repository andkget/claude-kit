# claude-kit

A personal Claude Code **marketplace** with one plugin, `workflow`: a stack-agnostic
engineering workflow plus a reusable project structure that travels across projects.

The split it enforces:

- **Portable (here):** the workflow commands and the house rules — they don't hardcode a
  stack, they read concrete commands from each project's `CLAUDE.md`.
- **Project-specific (per repo):** the `CLAUDE.md` that declares this project's description,
  setup/run/test commands, protected files, and base branch.

## What's inside

`plugins/workflow/`

- **Commands**
  - `/ship` — run the project's test gate, then branch → commit → push → PR → review →
    squash-merge.
  - `/write-tests` — add tests for uncovered behavior, then run the suite.
  - `/sync-docs` — promote durable decisions into `docs/decisions/` ADRs and `CLAUDE.md`.
  - `/feature-log` — append a session's decisions/progress/questions to the active
    `docs/plans/*.md`.
  - `/scaffold` — lay the reusable docs skeleton + a `CLAUDE.md` template into a repo.
- **Skill** `project-conventions` — the house rules: ADR discipline, living plan docs,
  docs-ship-with-code, git-by-risk, test discipline, gates that apply to the agent's output
  but never to the human's edits, and the `CLAUDE.md` contract.
- **Template** `templates/project-skeleton/` — what `/scaffold` copies: `CLAUDE.md`,
  `AGENTS.md` (generic, orients Codex), `.claude/settings.json` (generic git allowlist),
  `docs/decisions/` (README + ADR template), `docs/plans/` (README + `archive/`).

## Two coding tools

Projects are built **Claude-Code-first** (Claude Code reads `CLAUDE.md`). The generic
`AGENTS.md` keeps **Codex** usable as a token-backup and for code-review / bug-fixes — it
points Codex at the project's `CLAUDE.md` + `docs/decisions/`, so there's a single source
of truth and `CLAUDE.md` is never touched for Codex's sake. See the `project-conventions`
skill for the full house rule.

### Getting started with Codex

One-time, per machine: install the Codex CLI and sign in (Codex is included in a ChatGPT
Plus/Pro subscription — no separate API key). Then, in any repo that has been `/scaffold`ed:

```
codex            # run in the repo root; it auto-reads AGENTS.md
```

When to reach for it:

- **Code review** — when a branch/diff is ready, review it in a **read-only sandbox** so
  Codex looks without editing. A second model catches blind spots the author model missed.
- **Localized bug-fix** — when the bug is understood; keep the diff narrow.
- **Token-backup** — when the Claude Code usage window is spent and you want to keep going.

Switching tools mid-feature: the active `docs/plans/<feature>.md` is the hand-off — read it
before starting, update it (state + next steps) before stopping. **Git is the boundary:**
commit (and push, or use `git worktree` for parallel work) before you switch, so the next
tool picks up clean state. Pick the model by weight — a small/fast Codex model for chores,
the strongest for hard design — the same way you'd pick Opus vs. a lighter model.

## Install

From any project (or globally), register this repo as a marketplace, then install the plugin.

Local (works immediately, no GitHub needed):

```
/plugin marketplace add ~/Documents/Projects/claude-kit
/plugin install workflow@claude-kit
```

From GitHub (for other machines, once pushed):

```
/plugin marketplace add <your-user>/claude-kit
/plugin install workflow@claude-kit
```

## Bootstrap a new project

In the new repo:

```
/scaffold
```

Then fill in the bracketed sections of the generated `CLAUDE.md` (project description,
`## Setup` / `## Run` / `## Run the tests`, protected files). After that, `/ship`,
`/write-tests`, `/sync-docs`, and `/feature-log` are ready.

## Accumulating practices

This repo is the place rules accumulate: refine a command or convention here, commit, and
every project that has the plugin installed picks it up (re-run `/plugin marketplace update`
to pull the latest). Keep project-specific details in each repo's `CLAUDE.md`, not here.
