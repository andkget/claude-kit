# Public release of claude-kit

**Status:** shipped, archived. Durable decisions live in `docs/decisions/`; the repo's own
contract lives in `CLAUDE.md`.

## Goal

Open the repository publicly. It had been a private, single-author kit; going public meant it
had to be readable by someone with no context, carry a license, and — since its whole subject
is engineering conventions — visibly follow the conventions it ships.

## Scope

- Audit for secrets and personal data before anything becomes public.
- Unify commit authorship onto one address.
- License, plugin metadata (`license` / `repository` / `homepage` / `keywords`), README for an
  external reader.
- Dogfood: the repo gets its own `CLAUDE.md`, `docs/decisions/`, `docs/plans/`, and
  `.claude/settings.json`.
- A real test gate in CI, and eval cases for the behaviour the gate can't check.

## Decisions log

- **The test gate is `claude plugin validate --strict`, not a hand-rolled script.** It is the
  official checker, it covers manifests plus the commands and skills, and it needs no login —
  so CI runs it with no secrets at all. Verified against an empty `CLAUDE_CONFIG_DIR` before
  committing to it.
- **Evals stay out of CI.** `claude plugin eval` starts real Claude sessions: tokens and a
  login. It runs by hand before a release instead, and `CLAUDE.md` says so.
- **The four ADRs were reconstructed from existing commits, not invented.** Each one traces to
  the commit that made the decision (`fa78af5`/`cb582dc` → 0001, `d1c0fb6`/`a8df4e6` → 0002,
  `e44cfe5` → 0003, `c892ed1`/`a6deeaa` → 0004) and is dated accordingly. Backfilling an ADR
  with a plausible-sounding rationale nobody actually had would make the directory worthless.
- **"Never force-push the base branch" became an enforced `deny`**, in both this repo's
  `.claude/settings.json` and the scaffold template. It had been prose only, and prose is not
  a permission rule.
- **One factual correction, no change of substance.** The kit claimed Claude Code reads
  `AGENTS.md` as a fallback when `CLAUDE.md` is absent. It does not read `AGENTS.md` at all.
  The conclusion the kit drew from it — that adding `AGENTS.md` is free — survives, and is now
  stated for the right reason. Fixed in the skill and the skeleton's `AGENTS.md`.

## Progress

Shipped in one PR (`feature/public-release`) against a clean gate. Version bumped to `0.4.0`
per ADR 0004.

## Open questions

- The eval suite has four cases and covers the most distinctive rules (gate scope, test-gate
  ordering, kit-vs-project routing, no false firing). `/write-tests`, `/sync-docs`, and
  `/feature-log` have no case yet — worth adding when their wording next changes.
