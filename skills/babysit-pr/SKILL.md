---
name: babysit-pr
description: Take a pull request through its checks and reviews until it is ready to merge, fixing the real findings. Use when asked to babysit, watch, or monitor a PR.
license: MIT
---

# Babysit PR

Take one PR to **ready**, on its latest head:

- checks pass, or the repository has none;
- the automated reviews that run on their own have finished;
- every review thread is answered and resolved;
- the head contains the default branch, and GitHub reports it mergeable.

Merging stays a separate request.

## A pass

1. **Read** the PR's head SHA, checks, mergeability, reviews, comments, and review threads. Threads and their resolved state come from GraphQL (`reviewThreads`), not `gh pr view`. A bot reviewer may finish with a comment or a reaction instead of a review: read all three.
2. **Verify** each failure and finding against the code as it is now, including one raised on an older head. Rerun a failing check's own piece alone to tell a defect from a flake.
3. **Fix** a finding that is real and within what the PR is for, with the smallest change that removes it; a fix that deletes or narrows code beats one that adds a guard. Decline the rest, as a follow-up: a rare path outside the PR's purpose, a test that pins nothing, extra compatibility, a small cost.
4. **Push** the fixes as one update, having rerun what they touch. When the default branch has moved, merge it and rerun the repository's full verification first.
5. **Reply** on each thread with the fix or the reason you declined, then resolve it.

## Waiting

After a pass, wait for the PR's next change with your harness's PR watcher and make another pass when it wakes you: in T3 Code, `watch_pull_request`, then end your turn. With no watcher, `gh pr checks <n> --watch` waits for the checks alone: once they finish, report any review still running and stop, rather than polling for it. A wake that needs no action needs no message.

**One pass.** When your brief says an orchestrator watches the PR, make one pass and hand back with checks or a review still running. The waiting is the orchestrator's.

## Stop

Report instead of waiting when someone else closed or merged the PR, a human review is all that remains, infrastructure is down, or the same blocker survives two remedies. Name what is outstanding: a blocked PR is not ready.

## Report

The PR URL and head SHA, each check with its result, the findings fixed and those declined with their reasons, any thread still open, and whether the PR is ready.
