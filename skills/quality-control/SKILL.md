---
name: quality-control
description: "Put a spec's merged tickets through a quality pass: a review and fix PR, a test plan, a browser test run, and a fix PR for what the run finds, merging each PR once settled."
disable-model-invocation: true
license: MIT
---

# Quality Control

Run this once a spec's tickets have merged, each one already delivered and reviewed alone (typically by `tackle-issue`). Agents built those tickets independently, so this pass consolidates the spec as a whole in review, then tests it in the running app.

You orchestrate; subagents do the work. Your own hands only brief, watch, decide, ask the user, and merge.

**Scope** is what the user names: a spec (a parent issue and its sub-issues), PRs, or a commit range. Resolve it to the issues that specify the work, their merged PRs, and the range they span: `<base>..<head>`, where `<base>` is the default branch just before the first PR. Every brief carries the scope, pinned to the commit it was read at.

**Decisions** are the product choices the scope already records: decision sections in the issues, Deviations in the PR bodies, the user's replies. Every brief carries them as binding. A subagent that believes one is wrong lists it under **Open questions**, with a concrete scenario, and leaves the behaviour as it is.

| Role | Agent |
| --- | --- |
| Reviewer, planner, fixer | Claude `claude-opus-5-5`, effort `high` |
| Tester | Codex `gpt-6.1-sol`, reasoning effort `high` |

Start each subagent with `delegate_task` and the target above. The reviewer, planner and fixer get `isolation: "worktree"`. The tester runs where the plan's Environment section says: `isolation: "shared"` from the checkout it names, otherwise a worktree. If a named model is unavailable, tell the user and stop.

A brief names each skill to follow by the absolute path of its installed `SKILL.md`: the project's `.agents/skills/<name>/` or the global `~/.agents/skills/<name>/` (`npx skills list` shows both). `spec-review` and `implement` are user-invoked, so a subagent can only read them.

## Babysit

The subagent that blasts a PR owns it: it runs `babysit` on that PR, addresses every review comment and failing check, and hands back only once the PR is settled, ending with `PR <url>, head <sha>, checks and reviews <state>`.

**Settled**: checks pass (or the repo has none), the automated review finished on the current head, no unresolved actionable threads, and GitHub reports it mergeable.

A handback without that line is unfinished: send the same subagent a message to continue with `t3_thread_send`, so it keeps its context. Check a full handback against **Settled** yourself. Not settled yet: `watch_pull_request`, and send each new comment or failure to that same subagent; the fixes stay its job. Settled: merge with the repo's merge method (match recent merges on the default branch), `gh pr merge <n> --<method> --delete-branch --match-head-commit <sha>`, then `unwatch_pull_request` if you watched.

## Steps

1. **Review and consolidate.** Brief a reviewer subagent: run `spec-review` over the scope and its range `<base>..<head>`, publish with `blast-it`, and `babysit` the PR until it settles. Done when it hands back a settled PR, or reports that the review found nothing worth changing (skip to step 3, carrying any open questions).
2. **Merge** the review PR.
3. **Plan.** Brief a planner subagent: write a manual test plan with `test-plan` covering the scope and step 1's PR, pinned to the merged default-branch commit, saved outside the repository, with an observe case for each open question from step 1. Done when the plan file exists with case IDs, 📸 marks, observe cases, a restore checklist, and a report template.
4. **Test.** Brief the tester subagent: run the plan with `execute-test-plan` at that commit, writing the report and evidence beside the plan. Answer its questions with `t3_pending_request`. Done when the report gives every case a result and its clean-up section is filled.
5. **Decide.** Read the report and step 1's open questions, and sort each item:
   - A FAIL from a defect is a fix.
   - A FAIL where the plan expected what a recorded decision since changed is a plan correction; note it for the user.
   - An OBSERVED case, an open question, or a FAIL that disputes the spec is a product question. Put them all to the user in one round, each with its scenario, the options, and your recommendation. Each decision that asks for a change is a fix.
   - A BLOCKED case from the environment is noted for the user.

   No fixes: go to step 8.
6. **Fix.** Brief a fixer subagent with each fix as its spec (a FAIL's steps, expected vs actual and screenshot path; a decision with its scenario): follow `implement`, publish with `blast-it`, and `babysit` the PR until it settles. Done when it hands back a settled PR.
7. **Merge** the fix PR.
8. **Report** to the user: each PR merged (link and merge commit), review findings applied, the user's decisions, test counts (PASS/FAIL/BLOCKED/OBSERVED), fixes shipped, plan corrections, BLOCKED causes, and anything left behind in data or environment.
