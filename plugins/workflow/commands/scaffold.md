---
description: Bootstrap a new repo with the reusable docs structure (CLAUDE.md, ADRs, plans) and a generic git permission allowlist
---

Lay down the reusable project skeleton in the **current** repository, then help fill in the
project-specific parts. The skeleton is the portable "bones" the other commands rely on.

Steps:

1. **Copy the skeleton without overwriting anything that already exists:**

   ```bash
   cp -Rn "${CLAUDE_PLUGIN_ROOT}/templates/project-skeleton/." .
   ```

   This adds (only where missing): `CLAUDE.md`, `.claude/settings.json`,
   `docs/decisions/README.md` + `docs/decisions/0000-adr-template.md`,
   `docs/plans/README.md`, and `docs/plans/archive/`.

2. **Report what was created vs skipped** (skipped = already present), so the user knows
   nothing of theirs was clobbered.

3. **Tailor `CLAUDE.md` to this project.** The template declares the standard sections the
   workflow commands read; fill them in for this repo:
   - a one-paragraph **project description** (what it is, the stack);
   - **`## Setup`**, **`## Run`**, and **`## Run the tests`** — the concrete commands (these
     are what `/ship` and `/write-tests` execute);
   - the **protected / never-commit** list (e.g. `.env`, credentials, local DBs, large
     artifacts);
   - the **base branch** and git-by-risk policy if it differs from the defaults.

   Pull these from the conversation / repo when you can; ask the user only for what you
   can't infer. Do not invent commands — if a test runner isn't set up yet, leave a clearly
   marked TODO in `## Run the tests`.

4. Mention that `/ship`, `/write-tests`, `/sync-docs`, and `/feature-log` are now ready to
   use, and that the `project-conventions` skill documents the house rules.
