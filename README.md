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
  docs-ship-with-code, git-by-risk, test discipline, and the `CLAUDE.md` contract.
- **Template** `templates/project-skeleton/` — what `/scaffold` copies: `CLAUDE.md`,
  `AGENTS.md` (generic, orients Codex), `.claude/settings.json` (generic git allowlist),
  `docs/decisions/` (README + ADR template), `docs/plans/` (README + `archive/`).

## Two coding tools

Projects are built **Claude-Code-first** (Claude Code reads `CLAUDE.md`). The generic
`AGENTS.md` keeps **Codex** usable as a token-backup and for code-review / bug-fixes — it
points Codex at the project's `CLAUDE.md` + `docs/decisions/`, so there's a single source
of truth and `CLAUDE.md` is never touched for Codex's sake. See the `project-conventions`
skill for the full house rule.

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
