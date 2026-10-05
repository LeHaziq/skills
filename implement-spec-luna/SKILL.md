---
name: implement-spec-luna
description: "Implement a specification as one PR with Luna/max task agents and Sol/high review."
---

You have been provided a spec with tickets that describe its implementation.

The issue tracker should have been provided to you. If `docs/agents/issue-tracker.md` is missing, tell the user to run `/setup-matt-pocock-skills`.

The goal is one PR that implements the entire spec on a single integration branch. Treat the tickets as a task graph with blocking relationships. The current frontier contains every ticket whose blockers are complete.

## Agent routing

The coordinator runs in the current session on the user's selected model.

Run every exploration, implementation, merger, and review-fix sub-agent with `fork_turns: "none"`, `model: "gpt-5.6-luna"`, and `reasoning_effort: "max"`. Start independent agents in the background where possible.

Give each sub-agent a self-contained prompt with the repository root, assigned worktree and branch, role, ticket ownership, relevant repository instructions, completion criteria, and context pointers to the spec, tickets, research notes, and prerequisite commits. Tell it that other agents share the repository and that it must preserve unrelated changes.

Communicate through context pointers instead of copying material already available at those paths.

## Process

1. Read the spec, tickets, and issue-tracker instructions until the task graph and initial frontier are clear.

2. If tickets require codebase or documentation research, use a Luna/max exploration sub-agent. Store its Markdown notes outside the repository at a path available to later agents.

3. Record the PR base commit as the review fixed point. Create the integration branch. Open the draft PR after the first merge in step 5 creates commits ahead of the PR base, marked as closing the spec issue and its tickets.

4. Assign each frontier ticket to a Luna/max implementation sub-agent. Give each agent its own worktree and branch based on the current integration branch. Each agent must confirm that base, load `tdd` through the harness's skill-loading mechanism, implement only its ticket, run relevant checks, and commit its work. Before reporting done, merge the current integration tip into its branch and rerun affected checks. Report the final commit and verification results.

5. After an implementation agent finishes, use a Luna/max merger sub-agent to merge its work into the integration branch. If the integration branch advanced after the agent's final synchronization, first merge its current tip into the ticket branch and rerun affected checks. The merger must resolve conflicts without discarding unrelated work and confirm the ticket commit is reachable from the integration branch.

6. Recalculate the frontier after each merge. Start newly unblocked tickets up to the environment's safe concurrency limit. Continue until every ticket is merged.

7. On a clean integration branch, load `code-review-sol` through the harness's skill-loading mechanism with the base commit from step 3 and the complete spec. Give all review findings to one Luna/max implementation sub-agent in its own worktree and branch based on the integration branch. Merge its fixes into the integration branch after it runs the relevant checks and commits them.

8. Mark the PR ready for review.

9. Remove only the implementation worktrees created by this run, after their commits are merged into the integration branch.

Report the PR, completed tickets, merged commits, verification results, review findings, fixes, and worktree cleanup.
