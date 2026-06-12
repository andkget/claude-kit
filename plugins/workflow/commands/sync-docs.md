---
description: Promote durable decisions from this conversation into docs/decisions/ ADRs and CLAUDE.md — only if warranted
---

Review THIS conversation and update the project's durable docs **only if** something new
and worth recording emerged. Be conservative: if nothing qualifies, say "no doc changes
needed" and edit nothing. See the `project-conventions` skill for the ADR discipline.

What qualifies for an **ADR** (`docs/decisions/`):

- A significant, lasting design/architecture decision — a real choice between options with
  consequences. Not routine implementation detail.

What qualifies for **CLAUDE.md**:

- A change to how the project is set up, run, built, or tested; a new convention or process;
  a new gotcha worth warning the next session about.

Rules:

- ADRs are **immutable** once accepted. To change a past decision, add a NEW ADR that
  supersedes the old one (note the supersession in both files); never rewrite an accepted
  ADR.
- A new ADR uses the next sequential number + a kebab-case slug and follows the structure of
  existing ADRs / `docs/decisions/0000-adr-template.md` (Status/Date → Context → Options
  considered → Decision → Why → Consequences). Add a row to the index table in
  `docs/decisions/README.md`.
- Edit `CLAUDE.md` in place, in the relevant section.
- Docs ship in the SAME PR as the change they describe — these edits ride along with the
  feature's branch/PR, not a separate one.
- If a feature plan doc in `docs/plans/` references the new decision, link it to the ADR.

Before writing, list what you intend to add/change and why, then make the edits.
