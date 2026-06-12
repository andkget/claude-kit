# CLAUDE.md

<!--
This file is the contract the `workflow` plugin's commands read. Fill in the bracketed
parts for THIS project. The section headings marked "(commands read this)" are relied on
by /ship and /write-tests — keep them.
-->

[One paragraph: what this project is, the stack, and the high-level shape. Replace this.]

Design rationale lives in [docs/decisions/](docs/decisions/README.md); in-progress features
have living docs in [docs/plans/](docs/plans/README.md).

## Setup

```bash
[ one-time setup commands — e.g. create a virtualenv and install deps ]
```

## Run

```bash
[ how to run the app / the main entry points ]
```

## Run the tests   <!-- (commands read this) -->

```bash
[ the test command — this is the gate /ship runs and the suite /write-tests runs ]
```

- **Test isolation:** [how tests stay off real/production data — e.g. in-memory DB,
  throwaway sandbox. Tests must never touch real data or services.]

## Conventions

- **Base branch:** `main`.
- **Branch naming:** `feature/<task>`, `fix/<task>`, `docs/<task>`.
- **Git ceremony by risk:** code changes → branch + PR, squash-merge, auto-merge once the
  test gate is green and the diff is clean; docs-only/trivial → straight to the base branch.
  Never force-push `main`; `git push` asks for confirmation.
- **Protected / never-commit:** [`.env`, credentials, local databases, large artifacts — list
  the concrete paths/patterns for this project]. These stay gitignored and are never staged.
- Durable design decisions → an ADR in `docs/decisions/` (immutable; supersede, don't
  rewrite). Setup/run/test/process changes → update this file. Docs ride in the same PR as
  the code they describe.

> Full philosophy: the `project-conventions` skill (from the `workflow` plugin).

## Project commands (from the `workflow` plugin)

- `/ship` — test gate → branch → commit → push → PR → review → squash-merge.
- `/write-tests` — add tests for uncovered behavior, then run the suite.
- `/sync-docs` — promote durable decisions into ADRs / this file (same PR).
- `/feature-log` — append this session's decisions/progress/questions to the active
  `docs/plans/*.md`.
- `/scaffold` — (re)lay the docs skeleton in a repo.
