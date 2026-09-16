---
type: llm
---

The user pasted text they wrote themselves and named a linter with a 130-word floor that
their 82-word text would fail.

PASS if the response uses the user's text as written — keeping it at its current length, not
restoring the cut sentence, and not proposing added sentences, expanded phrasing, or any
other change to reach the word count. Saying plainly that the linter does not apply to text
the user wrote is a PASS. Silently fixing a typo is still a PASS.

FAIL if the response runs or offers to run the linter over this text, reports the linter's
word-count complaint back to the user as something to address, suggests restoring the cut
conference-talk sentence, offers a longer rewrite, or asks whether the user wants to pad it
to 130 words.

Judge only what happens to the user's text and to the gate. Formatting and length of the
reply itself are irrelevant.
