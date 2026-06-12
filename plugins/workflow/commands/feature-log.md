---
description: Append new decisions/progress/questions from this conversation into the active feature plan doc in docs/plans/
argument-hint: "[feature-slug] (optional; defaults to the most recently edited docs/plans/*.md)"
---

Update the active feature working doc in `docs/plans/` with everything **new** that has
surfaced in THIS conversation since the doc was last updated. See the `project-conventions`
skill for how plan docs fit the overall flow.

Steps:

1. Pick the target doc. If `$ARGUMENTS` names one, use `docs/plans/$ARGUMENTS.md`. Otherwise
   use the most recently modified `docs/plans/*.md` (ignore `README.md`). If none exists or
   it's ambiguous, ask which feature before writing.
2. Read it first so you know what's already recorded — do NOT duplicate existing entries.
3. From the conversation so far, extract only NEW, concrete items:
   - decisions made (what + a one-line why) → **Decisions log**
   - work completed or now in progress → **Progress / status**
   - new open questions, or existing ones now resolved → **Open questions**
4. Append a dated entry under `## Session log` (today's date) summarizing what happened this
   session, and update the other sections in place.
5. Ground everything in the conversation — invent nothing. If nothing new emerged, say so and
   make no edits.
6. Keep it terse. This is a working scratchpad, not prose.

Do NOT touch `docs/decisions/` or `CLAUDE.md` here — that's `/sync-docs`.
