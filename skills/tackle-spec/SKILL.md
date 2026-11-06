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

Run up to two ready tickets at once, or the number the user gives. Two tickets that each generate a numbered file, such as a database migration, run one after the other, since parallel ones collide on the number. Tell each implementer which files the other running ticket edits, so it merges the default branch and reverifies before handing back.

## Questions first

Once the graph is read, brief one auditor per open ticket, all at once, to check it as **Check** below says. Put every ticket's product questions to the user in one round, as `quality-control`'s **Babysit** section says to ask, and send each answer to the auditor that raised it. A question that only surfaces later goes to the user the same way, batched with any other still open. Done when every ticket's red flags have decisions and its body is published.

## Each ticket

1. **Check.** The auditor reads the ticket against the current default branch and returns only its **red flags**, each with a proposed fix:
   - a product question the spec cannot answer;
   - a premise the code contradicts, such as deleting something still in use, a layout the page does not have, or a pattern that does not exist;
   - a name, prop or export a later ticket relies on that the ticket gets wrong or leaves open.

   How to build the ticket stays with the implementer: the auditor writes no file plan, prompts or test lists. Once the user has answered, it writes the body with every red flag settled in a **Decisions** section and the rest left as written, to a file outside the repository; publish it with `gh issue edit <n> --body-file`. Done when every red flag has a decision. No red flags: the ticket goes as it is.

   Every body is checked before the tickets it builds on merge. When a ticket becomes ready, re-brief its auditor only if a merged PR it builds on lists Deviations that touch it.
2. **Tackle.** Brief an implementer subagent: follow `tackle-issue` on the ticket in its delegated mode, so it makes one follow-up pass and hands the PR back for you to watch. Its **Decisions** bind; the rest of the body is a plan, and where the code shows the plan wrong, the implementer departs from it and lists why under **Deviations** in the PR body. The ticket ships as one PR. Split it only when the user wants to read the PR by hand, and then stack the parts so each starts as soon as the one before is published. The implementer stops any dev server it started and releases any slot it took, leaves the merge to you, and ends with `PR <url>, head <sha>, checks and reviews <state>`.
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
