# 0001 — Conventions ship as an installable plugin, not as copies per repo

- Status: Accepted
- Date: 2026-06-11

## Context

A working set of engineering conventions — ADR discipline, living plan docs, a test-gated
git cycle — had accumulated inside a single project (`HhProject`), written into its
`CLAUDE.md` and its docs tree. They were good enough to want in the next project, and the
obvious move was to copy the files across.

Copying has a known end state: three repos hold three drifting versions of the same rule,
an improvement made in one never reaches the others, and there is no answer to "which copy
is right". The conventions also had to survive contact with unlike projects — a Python
service, a TypeScript CLI, a docs repo — so anything that named a test runner or a package
manager would only be reusable by accident.

## Options considered

1. **Copy the docs into each new repo** — zero infrastructure, immediate. Guarantees drift;
   every fix has to be re-applied by hand in N places, and in practice never is.
2. **A template repository** — `git clone` a starting point. Fixes the first day and nothing
   after it: a template gives no way to push an improved rule into repos already created
   from it.
3. **A Claude Code marketplace exposing one plugin** — conventions become a `SKILL.md` and
   slash commands installed into each project. One edit, then `plugin update`, and every
   project has the new rule.

## Decision

Package the conventions as a marketplace (`.claude-plugin/marketplace.json`) exposing one
plugin, `workflow`, holding five commands (`/ship`, `/write-tests`, `/sync-docs`,
`/feature-log`, `/scaffold`), the `project-conventions` skill, and a project skeleton that
`/scaffold` copies into a repo.

The split that makes this work is a contract: **the plugin is stack-agnostic and each
project's `CLAUDE.md` supplies the concrete commands.** Commands never hardcode a test
runner; they read a documented `## Run the tests` section from the consuming project and
execute whatever it declares. `/scaffold` lays down a `CLAUDE.md` template carrying exactly
the sections the commands look for.

## Why

Option 3 is the only one where an improvement propagates. It also forces the portability
constraint into the design rather than leaving it to discipline: a command that cannot name
a stack has to read one, and the reading mechanism is the `CLAUDE.md` contract. That
contract turns out to be independently useful — a project that declares its own
setup/run/test commands in one known place is easier for any agent to pick up, plugin or not.

The cost — that a rule now lives outside the repo that uses it — is paid back by an explicit
routing question ("would my other projects benefit from this?") documented in the skill, so
each new practice has an obvious home.

## Consequences

- Nothing under `plugins/workflow/` may name a language, framework, or test runner. A change
  that only makes sense for one kind of project belongs in that project.
- Projects gain a hard dependency on declaring `## Setup` / `## Run` / `## Run the tests` in
  their `CLAUDE.md`; a project that skips this gets commands that stop and ask.
- Improvements must be routed deliberately, not written where they were discovered. The
  `project-conventions` skill carries the "this project vs the shared kit" rule for that.
- Propagation is not automatic: an edit here reaches installed projects only through a
  version bump and a plugin update — see [ADR 0004](0004-a-kit-change-ships-only-with-a-version-bump.md).
