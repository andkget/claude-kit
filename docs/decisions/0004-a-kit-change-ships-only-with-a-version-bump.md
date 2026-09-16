# 0004 — A kit change ships only with a version bump

- Status: Accepted
- Date: 2026-08-24

## Context

The premise of [ADR 0001](0001-conventions-ship-as-a-plugin-not-copies.md) is that an edit
here propagates to every project that installed the plugin. It turned out not to, silently.

The `project-conventions` skill was edited and the marketplace refreshed, and projects kept
loading the previous text. Claude Code caches installed plugins at
`~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/`, **keyed by the plugin's
version**. With the version unchanged, the cache entry is already populated and no amount of
`marketplace update` replaces it. The failure is quiet: the commands still work, they just
enforce the old rule, and the only symptom is an agent citing a convention that was rewritten
weeks ago.

## Options considered

1. **Refresh the marketplace harder** — re-add it, or delete the cache directory by hand.
   Works once, relies on remembering an undocumented workaround forever, and does nothing for
   another machine.
2. **Drop `version` from `plugin.json`** so the version is derived elsewhere and cannot go
   stale. Gives up pinning and reproducibility, and makes "which rules is this project on?"
   unanswerable.
3. **Treat the version as part of the change** — any edit under `plugins/workflow/` is
   incomplete until `plugin.json`'s `version` is bumped in the same commit.

## Decision

A change to the kit is not shippable without a version bump in the **same commit**. Bumping
`plugins/workflow/.claude-plugin/plugin.json` is a step of the change, not a release chore
that follows it.

Pulling a change through, per machine, is two CLI commands:

```bash
claude plugin marketplace update claude-kit   # refresh the marketplace mirror
claude plugin update workflow@claude-kit      # install the new version (restart to apply)
```

`/plugin` is the interactive menu and takes no arguments — it is not the way a change is
pulled through. Both the README and the `project-conventions` skill state this.

## Why

The cache key is the mechanism, so the version is the only honest signal that the content
changed; option 3 makes the signal impossible to forget by attaching it to the commit rather
than to a later step. It also preserves what a version is for — a project can say which rule
set it is on, and an old version can be pinned or rolled back.

The quiet failure mode is what forces this to be a rule rather than a habit. A loud error
could be fixed when it appeared; a stale convention that still "works" can persist
indefinitely.

## Consequences

- Every PR touching `plugins/workflow/` shows a `version` change in its diff; a reviewer
  seeing one without it should reject it.
- Consumers need the two CLI commands above, per machine, before a change reaches them — the
  kit is not a live dependency and never updates itself.
- Semantics of the bump are informal: new or changed convention → minor, wording and fixes →
  patch. The requirement is that it changes, not that it is semantically perfect.
