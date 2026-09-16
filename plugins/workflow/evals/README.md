# Behaviour evals

These are [`claude plugin eval`](https://code.claude.com/docs/en/plugin-evals) cases for the
`workflow` plugin. `claude plugin validate` checks that the plugin's files are well-formed;
these check that the plugin actually steers Claude the way the conventions say it should.

```bash
# from the repository root
claude plugin eval ./plugins/workflow --ablation none
```

Drop `--ablation none` to also run a no-plugin baseline arm and see `Δ` — how much of each
result the plugin is responsible for, rather than the base model. Add `--judge-model sonnet`
if an `llm` grader's verdicts look unstable.

These runs start real Claude sessions, so they cost tokens and need a login. That is why they
are **not** in CI — CI runs `claude plugin validate --strict`, which needs neither. Run the
evals by hand before releasing a version whose wording could shift behaviour.

## The cases

| Case | The convention under test |
|---|---|
| `ship-runs-the-test-gate-first` | `/ship` runs the project's documented test command before any commit, and branches instead of committing to the base branch ([ADR 0001](../../../docs/decisions/0001-conventions-ship-as-a-plugin-not-copies.md) — the command reads the test command from `CLAUDE.md` rather than knowing it) |
| `human-edit-is-not-regated` | A quality gate covers the agent's output, never text the human wrote ([ADR 0003](../../../docs/decisions/0003-quality-gates-cover-the-agents-output.md)) |
| `reusable-convention-goes-to-the-kit` | A generally-useful practice is contributed back to the kit with a version bump, not buried in one project ([ADR 0004](../../../docs/decisions/0004-a-kit-change-ships-only-with-a-version-bump.md)) |
| `no-false-fire` | An ordinary technical question is answered directly — the workflow commands do not fire on unrelated work |

## Writing a case

Each case directory holds a `prompt.md` (frontmatter: run limits and allowed tools; body: the
message Claude receives) and one or more `graders/*.md`. Two habits keep the suite honest:

- **Phrase the prompt the way a user would type it**, never naming the command. A case that
  says "run /ship" tests nothing but string matching.
- **Grade the result and the route.** One grader on what Claude produced (`llm` on the final
  message, or `regex` over a file) and one on how it got there (`tool_used`, `tool_order`).

Every run starts in an empty workspace with no `CLAUDE.md` and no project settings loaded, so
each case carries the context it needs inside the prompt itself. `results/` is written per run
and gitignored.
