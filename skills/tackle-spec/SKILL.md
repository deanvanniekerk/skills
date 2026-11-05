---
name: tackle-spec
description: "Deliver a spec issue end to end, or one big issue with no parent: tighten and tackle each ticket with tackle-issue in a subagent, merging each PR once settled, then run quality-control over the whole of it."
disable-model-invocation: true
license: MIT
---

# Tackle Spec

You take one spec, a parent issue and its sub-issues, from open tickets to a merged, quality-checked whole. An issue with no sub-issues is a spec of one ticket: the same steps run on that issue alone, for a big ticket that earns the full pass. Subagents tighten and tackle the tickets; your own hands only sequence, brief, ask the user, merge, and finish with quality control.

| Role | Agent |
| --- | --- |
| Auditor, implementer | Claude `claude-opus-5-5`, effort `high` |

`quality-control` brings its own roles. Start each subagent with `delegate_task`, `isolation: "worktree"`, and the target above. If a named model is unavailable, tell the user and stop. A brief names each skill to follow by the absolute path of its installed `SKILL.md`: the project's `.agents/skills/<name>/` or the global `~/.agents/skills/<name>/`.

Settle and merge every PR as `quality-control`'s **Babysit** section says: the subagent that opened it fixes what review and CI raise, you watch it, an unfinished handback resumes the same subagent, and you merge with the repository's own merge method once it is **Settled**.

## The ticket graph

Read the parent and every sub-issue (`gh api repos/<owner>/<repo>/issues/<n>/sub_issues`), with the blocking edges each declares: GitHub's blocked-by links (`gh api repos/<owner>/<repo>/issues/<n>/dependencies/blocked_by`), or "Blocked by #n" in its body. Closed sub-issues are done. A ticket is **ready** once every ticket blocking it has merged. Done when every open sub-issue sits in the graph; a ticket whose order the issues do not settle is a question for the user. With no sub-issues, the graph is the issue itself.

Run up to two ready tickets at once, or the number the user gives. Tell each implementer which files the other running ticket edits, so it merges the default branch and reverifies before handing back.

## Each ticket

1. **Tighten.** Brief an auditor subagent on the ticket. It:
   - re-checks every file, line and name the ticket cites against the current default branch, since earlier tickets have merged since it was written;
   - turns every open choice ("either", "prefer", "if needed") into a decision;
   - writes the tightened body to a file outside the repository, and returns the product questions the spec cannot answer.

   Put the questions to the user, batched with any other ready ticket's. Send the answers to the same auditor to record as decisions in the body. Then publish it with `gh issue edit <n> --body-file`. Done when the ticket has no open choice left and its references match the default branch.
2. **Tackle.** Brief an implementer subagent: follow `tackle-issue` on the ticket, list any mismatch with the ticket under **Deviations** in the PR body, leave the merge to you, and end with `PR <url>, head <sha>, checks and reviews <state>`.
3. **Merge** the PR once settled. Confirm the merge closed the ticket; if it did not, close it with a comment naming the PR.

After each merge, start whichever tickets became ready.

## After the last ticket

Read `quality-control`'s `SKILL.md` and follow it yourself, with the spec as its scope. The user's request to tackle the spec is the request to run it.

## Report

Tell the user:
- each ticket, with its PR and merge commit;
- the decisions they made along the way;
- `quality-control`'s report.

Leave the parent issue open: closing the spec is the user's call. A single issue closes with its own PR, as any ticket does.
