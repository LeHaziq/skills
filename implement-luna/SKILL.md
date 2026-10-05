---
name: implement-luna
description: "Implement a spec or tickets in the current Luna/max session, then run the Sol review."
---

Implement the user's spec or tickets directly in the current session. This workflow assumes the session uses `gpt-5.6-luna` with `max` reasoning effort.

## Process

1. Capture the review baseline before any implementation work with `git rev-parse HEAD`.
2. Read the full task, spec or ticket contents, repository instructions, current branch, and working-tree state. Preserve unrelated changes.
3. Implement only the supplied work. Use `$tdd` where possible at pre-agreed seams.
4. Run typechecking and focused tests during implementation, then run the full test suite once at the end.
5. Commit the completed work to the current branch.
6. Invoke `$code-review-sol` with the baseline SHA from step 1 and the full spec or ticket contents.

Report the implementation commit, verification results, and the two review axes.
