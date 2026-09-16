# claude-kit

[![validate](https://github.com/andkget/claude-kit/actions/workflows/validate.yml/badge.svg)](https://github.com/andkget/claude-kit/actions/workflows/validate.yml)

A Claude Code **marketplace** with one plugin, `workflow`: an engineering workflow that
travels across projects instead of being copied into each one.

The idea is a split. The workflow — how a change gets tested, branched, reviewed, merged, and
documented — is the same in every repo, so it lives here once and is installed. What differs
per repo is only the concrete commands, so each project declares those in its own `CLAUDE.md`
and the commands read them. Nothing in this repo names a language, a framework, or a test
runner.

- **Portable (here):** the commands and the house rules.
- **Project-specific (per repo):** a `CLAUDE.md` declaring the project's description,
  setup/run/test commands, protected files, and base branch.

Manifests and components are validated in CI on every push (`claude plugin validate
--strict`); the plugin's actual behaviour is checked with
[eval cases](plugins/workflow/evals/README.md). The reasoning behind the design is in
[docs/decisions/](docs/decisions/README.md).

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
  `AGENTS.md` (generic, orients Codex), `.claude/settings.json` (git allowlist that denies
  force-push outright), `docs/decisions/` (README + ADR template), `docs/plans/` (README +
  `archive/`).

## What `/ship` actually does

It has no idea what stack it's in. It reads the project's `CLAUDE.md`, then:

1. Runs the test command that `CLAUDE.md` declares under `## Run the tests`. If none is
   documented, it stops and asks — it never guesses one. Anything red stops the run here.
2. If a durable decision was made this session, runs `/sync-docs` first so the doc edits ride
   in the same PR as the code.
3. Branches (`feature/` · `fix/` · `docs/`) — never commits to the base branch.
4. Stages files by name, never `git add -A`, and never anything on the project's
   protected/never-commit list.
5. Pushes, opens the PR if one doesn't exist, self-reviews the diff, fixes findings on the
   same branch.
6. Squash-merges once the gate is green and the diff is clean — pausing instead if the diff
   is risky, tests were skipped, or the change touches something irreversible.

Ceremony scales with risk: docs-only changes may go straight to the base branch. That rule,
and the reasoning behind each step, is in the `project-conventions` skill.

## Two coding tools

Projects are built **Claude-Code-first** (Claude Code reads `CLAUDE.md`). The generic
`AGENTS.md` keeps **Codex** usable as a token-backup and for code-review / bug-fixes — it
points Codex at the project's `CLAUDE.md` + `docs/decisions/`, so there's a single source
of truth and `CLAUDE.md` is never touched for Codex's sake. See
[ADR 0002](docs/decisions/0002-claude-code-first-codex-as-second-tool.md) for why the file is
generic, and the `project-conventions` skill for the full house rule.

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

From GitHub:

```
/plugin marketplace add andkget/claude-kit
/plugin install workflow@claude-kit
```

From a local clone (works offline, and what you want if you're editing the kit):

```
/plugin marketplace add /path/to/claude-kit
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

## Developing the kit

The repo follows the conventions it ships — it has its own [`CLAUDE.md`](CLAUDE.md),
[ADRs](docs/decisions/README.md), and plan docs, and `/ship` works on it like any other
project. Its test gate is:

```bash
claude plugin validate . --strict && claude plugin validate ./plugins/workflow --strict
```

Behaviour changes are checked against the [eval suite](plugins/workflow/evals/README.md)
before release:

```bash
claude plugin eval ./plugins/workflow --ablation none
```

### Accumulating practices

This repo is where rules accumulate: refine a command or convention here, commit, and every
project that has the plugin installed picks it up. Keep project-specific details in each
repo's `CLAUDE.md`, not here.

**Bump `plugins/workflow/.claude-plugin/plugin.json` in the same commit as the change.** The
installed-plugin cache (`~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/`) is keyed
by that version: without a bump, projects keep serving the old files no matter how many times
the marketplace is refreshed — see
[ADR 0004](docs/decisions/0004-a-kit-change-ships-only-with-a-version-bump.md). Then, per
machine:

```bash
claude plugin marketplace update claude-kit   # refresh the marketplace mirror
claude plugin update workflow@claude-kit      # install the new version (restart to apply)
```

`/plugin` is the interactive menu and takes no arguments — the CLI commands above are what
actually pull a change through.

## License

MIT — see [LICENSE](LICENSE).
