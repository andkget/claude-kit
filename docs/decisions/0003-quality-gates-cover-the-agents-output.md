# 0003 — Quality gates cover the agent's output, not the human's edits

- Status: Accepted
- Date: 2026-08-24

## Context

Some projects run an automated gate over generated content: a prose linter, a style
checklist, a script the agent re-runs until it exits clean. The gate exists to keep the
agent's own drafts honest, and for that it works.

It stopped working the moment a human edit entered the same file. In the résumé repo, the
cover-letter linter enforced a 130-word floor; the author deliberately cut a sentence, the
file fell under the floor, and the agent put the sentence back to make the metric green.
The tool had started arguing with its owner — and the metric won.

## Options considered

1. **Gate everything in the file** — one rule, no ambiguity about what is checked. Produces
   exactly the failure above: the agent overrides deliberate human edits to satisfy a number.
2. **Drop the gate** — the human is always right, so stop measuring. Throws away the reason
   the gate existed; the agent's own drafts go unchecked.
3. **Scope the gate by authorship** — the gate has authority over text the agent produced and
   none over text the human wrote.

## Decision

A quality gate applies to the **agent's** output only.

- The human pastes their own version, or edits the file by hand → take it **as-is**. At most
  fix outright typos and grammar, silently.
- Do not run the gate over their text, do not report its complaints back to them, and never
  edit their words to satisfy a metric — no padding to reach a minimum length, no restoring
  content they deliberately cut.
- A targeted rewrite they asked for ("redo the second paragraph") is the agent's text again,
  so the gate applies to it.

## Why

A human edit is a decision, not a draft. A gate that starts correcting the author has
inverted who works for whom, and it reads as the tool arguing with its owner.

Authorship is also the only line that can actually be drawn at the moment of the edit — "is
this good writing?" cannot be, but "did I write this or did they?" always can. Option 3 keeps
the gate's original value on drafts while making it structurally incapable of the résumé
failure.

## Consequences

- Every project that runs a generated-content gate has to track authorship of the text it
  checks, at least well enough to answer "did the human touch this?".
- Gate output on human-authored text is not just ignored, it is unreported — surfacing the
  complaints is a softer version of the same inversion.
- A requested rewrite flips the scope back, so the boundary has to be re-evaluated per edit
  rather than set once per file.
- This is a general house rule, so it lives in `project-conventions` and reaches every
  project that installs the plugin, not only ones that happen to have a linter today.
