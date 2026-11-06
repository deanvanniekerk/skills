---
name: quality-control
description: "Put a spec's merged tickets through a quality pass: a review and fix PR, a test plan, a browser test run, and a fix PR for what the run finds, merging each PR once settled."
disable-model-invocation: true
license: MIT
---

# Quality Control

Run this once a spec's tickets have merged, each one already delivered and reviewed alone (typically by `tackle-issue`). Agents built those tickets independently, so this pass consolidates the spec as a whole in review, then tests it in the running app.

You orchestrate; subagents do the work. Your own hands only brief, watch, decide, ask the user, and merge.

**Scope** is what the user names: a spec (a parent issue and its sub-issues), a single issue, PRs, or a commit range. Resolve it to the issues that specify the work, their merged PRs, and the range they span: `<base>..<head>`, where `<base>` is the default branch just before the first PR. Every brief carries the scope, pinned to the commit it was read at.

**Decisions** are the product choices the scope already records: decision sections in the issues, Deviations in the PR bodies, the user's replies. Every brief carries them as binding. A subagent that believes one is wrong lists it under **Open questions**, with a concrete scenario, and leaves the behaviour as it is.

| Role | Agent |
| --- | --- |
| Reviewer, planner, fixer | Claude `claude-opus-5-5`, effort `high` |
| Tester | Codex `gpt-6.1-sol`, reasoning effort `high` |

Start each subagent with `delegate_task` and the target above. The reviewer, planner and fixer get `isolation: "worktree"`. The tester runs where the plan's Environment section says: `isolation: "shared"` from the checkout it names, otherwise a worktree, one per lane. If a named model is unavailable, tell the user and stop. Every brief tells a subagent that starts a local environment, such as a dev server or a database slot, to tear it down before handing back.

A brief names each skill to follow by the absolute path of its installed `SKILL.md`: the project's `.agents/skills/<name>/` or the global `~/.agents/skills/<name>/` (`npx skills list` shows both). `spec-review` and `implement` are user-invoked, so a subagent can only read them.

## Babysit

The subagent that blasts a PR owns its fixes: it makes one `babysit` pass on that PR, as `tackle-issue`'s delegated mode says, addresses every review comment and failing check already there, and hands back ending with `PR <url>, head <sha>, checks and reviews <state>`. Waiting is yours: T3 lets no delegated subagent watch or poll a PR, so a handback with checks or the automated review still running is complete.

**Settled**: checks pass (or the repo has none), the automated review finished on the current head, no unresolved actionable threads, and GitHub reports it mergeable.

A handback without the PR line is unfinished. One that says it is waiting on its own background command resumes by itself when that command finishes: leave it. One that stopped with nothing running gets a `t3_thread_send` to continue, so it keeps its context. Check a full handback against **Settled** yourself. Not settled yet: `watch_pull_request`, and send each new comment or failure to that same subagent; the fixes stay its job. Settled: merge with the repo's merge method (match recent merges on the default branch), `gh pr merge <n> --<method> --delete-branch --match-head-commit <sha>`, then `unwatch_pull_request` if you watched.

**Asking the user.** While any subagent is still running, put questions to the user in your reply rather than a blocking question tool, so handbacks still reach you and settled PRs keep merging while the user decides.

## Steps

1. **Review and consolidate.** Brief a reviewer subagent: run `spec-review` over the scope and its range `<base>..<head>`, publish with `blast-it`, and `babysit` it as **Babysit** says. Done when its PR is **Settled**, or it reports that the review found nothing worth changing (skip to step 3, carrying any open questions).
2. **Merge** the review PR.
3. **Plan.** Brief a planner subagent: write a manual test plan with `test-plan` covering the scope and step 1's PR, pinned to the merged default-branch commit, saved outside the repository, with an observe case for each open question from step 1. Done when the plan file exists with case IDs, 📸 marks, observe cases, a teardown, lanes when it has more than about 60 cases, and a report template.
4. **Test.** Brief one tester per lane, up to three at once, each on its own disposable environment (a plan without lanes is one lane): run that lane with `execute-test-plan` at that commit, writing its report and evidence beside the plan. Answer their questions with `t3_pending_request`. Done when every case has a result across the lane reports and every tester's environment is torn down.
5. **Decide.** Read the report and step 1's open questions, and sort each item:
   - A FAIL from a defect is a fix.
   - A FAIL where the plan expected what a recorded decision since changed is a plan correction; note it for the user.
   - An OBSERVED case, an open question, or a FAIL that disputes the spec is a product question. Put them all to the user in one round, each with its scenario, the options, and your recommendation. Each decision that asks for a change is a fix.
   - A BLOCKED case from the environment is noted for the user.

   A defect FAIL needs no answer: brief the fixer on those at once (step 6) and send it the user's change decisions when they arrive. No fixes: go to step 8.
6. **Fix.** Brief a fixer subagent with each fix as its spec (a FAIL's steps, expected vs actual and screenshot path; a decision with its scenario): follow `implement`, publish with `blast-it`, and `babysit` it as **Babysit** says. Decisions that arrive later go to the same fixer, on the same PR. Done when its PR is **Settled** with every fix in it.
7. **Merge** the fix PR.
8. **Report** to the user: each PR merged (link and merge commit), review findings applied, the user's decisions, test counts (PASS/FAIL/BLOCKED/OBSERVED), fixes shipped, plan corrections, BLOCKED causes, and anything left behind in data or environment.
