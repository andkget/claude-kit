# CLAUDE.md

This repository is a **Claude Code marketplace** holding one plugin, `workflow`: a
stack-agnostic engineering workflow (five commands + the `project-conventions` skill) and a
project skeleton that `/scaffold` copies into other repos. There is no application code —
the deliverable is Markdown and JSON that Claude Code loads as plugin components, so the
"build" is schema validation and the "behaviour tests" are eval cases.

Design rationale lives in [docs/decisions/](docs/decisions/README.md); in-progress features
have living docs in [docs/plans/](docs/plans/README.md). This file follows the same
`CLAUDE.md` contract the plugin asks of every project it scaffolds.

## Setup

```bash
npm install -g @anthropic-ai/claude-code   # provides the `claude` CLI used below
```

## Run

Register this checkout as a marketplace and install the plugin from it:

```bash
claude plugin marketplace add .
claude plugin install workflow@claude-kit
claude plugin details workflow            # confirm the component inventory
```

## Run the tests   <!-- (commands read this) -->

```bash
claude plugin validate . --strict && claude plugin validate ./plugins/workflow --strict
```

This is the gate `/ship` runs. It needs no login and no credentials, and it is the same
command CI runs on every push and PR (`.github/workflows/validate.yml`). `--strict` fails on
warnings, not just errors, so unrecognized fields and missing metadata break the build.

- **Behaviour evals (manual, before a release):** `claude plugin eval ./plugins/workflow
  --ablation none`. These run real Claude sessions, so they cost tokens and need a login —
  that is why they stay out of CI. Run them whenever a command or the skill changes wording
  that could shift behaviour. See [plugins/workflow/evals/README.md](plugins/workflow/evals/README.md).
- **Test isolation:** eval cases run in an empty throwaway workspace the runner creates, and
  carry their context inside the prompt. No case may touch this repo, real credentials, or a
  real remote.

## Conventions

- **Base branch:** `main`.
- **Branch naming:** `feature/<task>`, `fix/<task>`, `docs/<task>`.
- **Git ceremony by risk:** changes to plugin components (commands, skill, templates,
  manifests) → branch + PR, squash-merge, auto-merge once the gate is green and the diff is
  clean; docs-only/trivial → straight to `main`. Never force-push `main` — `.claude/settings.json`
  denies it outright; `git push` asks for confirmation.
- **Protected / never-commit:** `plugins/workflow/evals/results/` (scored eval output,
  regenerated per run), `CLAUDE.local.md`, `.claude/settings.local.json`. All are gitignored
  and never staged.
- **Bump `plugins/workflow/.claude-plugin/plugin.json` `version` in the same commit as any
  change under `plugins/workflow/`.** The installed-plugin cache is keyed by version — without
  a bump, projects keep serving the old files no matter how often the marketplace is
  refreshed. This is [ADR 0004](docs/decisions/0004-a-kit-change-ships-only-with-a-version-bump.md).
- Durable design decisions → an ADR in `docs/decisions/` (immutable; supersede, don't
  rewrite). Setup/run/test/process changes → update this file. Docs ride in the same PR as
  the change they describe.
- **Portability is the point.** Nothing here may hardcode a stack, a language, or a test
  runner. Commands read concrete commands from each consuming project's `CLAUDE.md`
  ([ADR 0001](docs/decisions/0001-conventions-ship-as-a-plugin-not-copies.md)). If a change
  would only make sense in one kind of project, it belongs in that project, not here.
- **Two tools:** Claude Code is primary and reads this file; the generic `AGENTS.md` in the
  skeleton orients Codex and points back at `CLAUDE.md`
  ([ADR 0002](docs/decisions/0002-claude-code-first-codex-as-second-tool.md)).

> Full philosophy: the `project-conventions` skill, which this repo ships in
> `plugins/workflow/skills/`. It is the authority; this file only declares the concrete
> commands.

## Repository layout

- `.claude-plugin/marketplace.json` — the marketplace manifest listing the `workflow` plugin.
- `plugins/workflow/commands/*.md` — the five slash commands.
- `plugins/workflow/skills/project-conventions/SKILL.md` — the house rules.
- `plugins/workflow/templates/project-skeleton/` — what `/scaffold` copies into a repo.
- `plugins/workflow/evals/` — behaviour eval cases for `claude plugin eval`.

## Project commands (from the `workflow` plugin, installed from this repo)

- `/ship` — test gate → branch → commit → push → PR → review → squash-merge.
- `/write-tests` — add tests for uncovered behavior, then run the suite.
- `/sync-docs` — promote durable decisions into ADRs / this file (same PR).
- `/feature-log` — append this session's decisions/progress/questions to the active
  `docs/plans/*.md`.
- `/scaffold` — (re)lay the docs skeleton in a repo.
