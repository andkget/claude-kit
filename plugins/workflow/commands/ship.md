---
description: Run the project's test gate, then take the current changes through the git/GitHub cycle (commit → push → PR → review → merge)
---

Take the current changes through the project's git/GitHub workflow. Read the project's
`CLAUDE.md` first for the concrete commands and rules; this command is stack-agnostic and
defers to what `CLAUDE.md` declares. **Never commit directly to the base branch** (default
`main` unless `CLAUDE.md` says otherwise). See the `project-conventions` skill for the
philosophy behind the steps.

1. **Test gate (must pass before anything else):**
   - Run the project's **test command** as documented in `CLAUDE.md` (look for a
     `## Run the tests` / `## Test` section). If the project has no test command
     documented, ask the user for it (and offer to record it in `CLAUDE.md`).
   - If anything fails, STOP and fix or report it before committing.
2. **Docs in sync:**
   - If a durable decision was made this session, run `/sync-docs` first so the doc edits
     ride along in the SAME PR.
3. **Branch:**
   - If on the base branch, create a branch: `feature/<task>` (features), `fix/<task>`
     (bugfixes), or `docs/<task>` (docs-only). If already on a task branch, stay on it —
     fixes from review go on the SAME branch.
4. **Commit:**
   - Review `git status` / `git diff`. Stage specific files by name (not `git add -A`).
     Never stage anything in the project's **protected/never-commit** list (e.g. `.env`,
     credentials, local databases, large artifacts) — see `CLAUDE.md`.
   - Write a concise message focused on the "why".
5. **Push:**
   - `git push -u origin <branch>`. If project settings make push prompt for confirmation,
     that's expected.
6. **PR:**
   - If no PR exists for the branch, `gh pr create --base <base> --title "..." --body "..."`.
     If one already exists, the push just updated it — don't open a second PR.
7. **Review:**
   - Give a brief self-review of the diff (or run the project's review flow) and address
     findings on the SAME branch.
8. **Merge — auto after green tests:**
   - Once the test gate is green and the diff is clean, merge without asking:
     `gh pr merge <num> --squash --delete-branch`, then sync the base branch
     (`git checkout <base> && git pull`).
   - Exception: pause and flag first if the diff looks risky, tests are red/skipped, or the
     change touches something irreversible/shared beyond this repo.

> Scale ceremony by risk (per `project-conventions`): docs-only/trivial changes may go
> straight to the base branch per the project's stated policy; code changes always take a
> branch + PR.
