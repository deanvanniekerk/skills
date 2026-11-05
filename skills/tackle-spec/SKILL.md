---
name: tackle-spec
description: "Deliver a spec issue end to end, or one big issue with no parent: check each ticket for red flags, tackle it with tackle-issue in a subagent, merging each PR once settled, then run quality-control over the whole of it."
disable-model-invocation: true
license: MIT
---

# Tackle Spec

You take one spec, a parent issue and its sub-issues, from open tickets to a merged, quality-checked whole. An issue with no sub-issues is a spec of one ticket: the same steps run on that issue alone, for a big ticket that earns the full pass. Subagents check and tackle the tickets; your own hands only sequence, brief, ask the user, merge, and finish with quality control.

| Role | Agent |
| --- | --- |
| Auditor, implementer | Claude `claude-opus-5-5`, effort `high` |

`quality-control` brings its own roles. Start each subagent with `delegate_task`, `isolation: "worktree"`, and the target above. If a named model is unavailable, tell the user and stop. A brief names each skill to follow by the absolute path of its installed `SKILL.md`: the project's `.agents/skills/<name>/` or the global `~/.agents/skills/<name>/`.

Settle and merge every PR as `quality-control`'s **Babysit** section says: the subagent that opened it fixes what review and CI raise, you watch it, an unfinished handback resumes the same subagent, and you merge with the repository's own merge method once it is **Settled**.

## The ticket graph

Read the parent and every sub-issue (`gh api repos/<owner>/<repo>/issues/<n>/sub_issues`), with the blocking edges each declares: GitHub's blocked-by links (`gh api repos/<owner>/<repo>/issues/<n>/dependencies/blocked_by`), or "Blocked by #n" in its body. Closed sub-issues are done. A ticket is **ready** once every ticket blocking it has merged. Done when every open sub-issue sits in the graph; a ticket whose order the issues do not settle is a question for the user. With no sub-issues, the graph is the issue itself.

Run up to two ready tickets at once, or the number the user gives. Tell each implementer which files the other running ticket edits, so it merges the default branch and reverifies before handing back.

## Each ticket

1. **Check.** Brief an auditor subagent to read the ticket against the current default branch, since earlier tickets have merged since it was written, and return only its **red flags**, each with a proposed fix:
   - a product question the spec cannot answer;
   - a premise the code contradicts, such as deleting something still in use, a layout the page does not have, or a pattern that does not exist;
   - a name, prop or export a later ticket relies on that the ticket gets wrong or leaves open.

   How to build the ticket stays with the implementer: the auditor writes no file plan, prompts or test lists. Put the product questions to the user, batched with any other ready ticket's, and send the answers to the same auditor. It writes the body with every red flag settled in a **Decisions** section and the rest left as written, to a file outside the repository; publish it with `gh issue edit <n> --body-file`. Done when every red flag has a decision. No red flags: the ticket goes as it is.
2. **Tackle.** Brief an implementer subagent: follow `tackle-issue` on the ticket. Its **Decisions** bind; the rest of the body is a plan, and where the code shows the plan wrong, the implementer departs from it and lists why under **Deviations** in the PR body. It leaves the merge to you and ends with `PR <url>, head <sha>, checks and reviews <state>`.
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
