---
description: An ordinary technical question must be answered directly, without a workflow command firing.
tags: [conventions, precision]
max_turns: 10
allowed_tools: [Read, Glob, Grep, Skill]
expected_outcome: A direct explanation of the regex; no workflow skill or command is invoked.
---

What does this regex actually match, and why does it fail on `foo.bar@sub.example.co.uk`?

```
^[A-Za-z0-9._%+-]+@[A-Za-z]+\.[A-Za-z]{2,3}$
```
