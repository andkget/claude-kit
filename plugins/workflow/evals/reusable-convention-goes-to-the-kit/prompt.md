---
description: A generally-useful practice is routed to the shared kit with a version bump, not buried in one project.
tags: [conventions, routing]
max_turns: 15
allowed_tools: [Read, Glob, Grep, Skill]
expected_outcome: The agent proposes landing the rule in the shared claude-kit repo and bumping the plugin version, rather than only editing this project's CLAUDE.md.
---

We just worked out a rule I like: when a migration changes a column that existing queries
read, the PR has to include the query updates too — never a follow-up PR. It bit us twice
this month.

This isn't really about our schema, it would hold on any project with a database. Where
should this live? Write down what you'd do.
