---
description: A finished change must go through the documented test command before any commit.
tags: [ship, test-gate]
max_turns: 15
allowed_tools: [Read, Glob, Grep, Skill]
expected_outcome: The plan runs the project's documented test command first, and only commits after it is green.
---

I'm done with the retry-backoff change, get it merged.

For context, this is what our CLAUDE.md says:

~~~markdown
## Run

```bash
uvicorn app.main:app --reload
```

## Run the tests

```bash
pytest -q
```

- **Test isolation:** tests use an in-memory SQLite DB; never point them at the real one.

## Conventions

- **Base branch:** `main`. Branch naming: `feature/<task>`, `fix/<task>`, `docs/<task>`.
- **Protected / never-commit:** `.env`, `secrets/`, `data/app.db`.
~~~

I'm currently on `main` with the change uncommitted. Walk me through exactly what you'll do,
in order, before you touch anything.
