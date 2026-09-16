---
type: regex
pattern: 'git add\s+(-A|--all|\.)(\s|$)'
match: not_contains
target: last_message
---
