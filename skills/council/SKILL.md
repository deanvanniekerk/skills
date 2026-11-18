---
name: council
description: Put an idea, plan, design, or piece of work before a council of models from different providers, then weigh their verdicts and revise. Use when the user says council, or asks several models to judge something.
license: MIT
---

# Council

A council is two or more models from different providers judging one **subject** independently. You are the **chair**: you ready the subject, write the brief, weigh the verdicts, decide, and revise. The subject is anything an agent can be shown: an idea, a plan or spec, game mechanics, a UI in the running app, an architecture, code, copy.

## 1. Seat

One member per provider, at reasoning effort `high`. Members the user names are the council. Otherwise seat the newest Claude Opus and the newest Codex GPT Astra the host lists (in T3 Code, `orchestrator_capabilities`), plus the flagship of any further provider it offers. With fewer than two providers reachable, or a named member missing, report it instead of substituting: two seats from one provider give one opinion twice.

## 2. Ready the subject

Convene on the current version. Fix the issues you already know of, address outstanding comments, and run the relevant checks first; a verdict on stale work is spent on what you would have fixed anyway. When the user wants the subject replaced rather than improved, skip fixes to what is being replaced.

Collect **evidence** members can open for themselves, as file paths, never your paraphrase:

- **Idea or plan** living only in chat: write its full text to a file in your scratchpad.
- **UI**: PNG screenshots at phone and desktop width, one per state that matters (empty, loading, error, dark mode), and the URL when the app runs locally.
- **Game mechanics**: the core loop, the player's goal, every number (costs, rates, odds, timers), and one worked turn or session played through.
- **Code**: the base ref and the diff range.

Done when a member holding only the brief and these files could judge the subject as well as you could.

## 3. Brief

One brief, word for word the same for every member; no member sees another's verdict. It carries:

- the subject, and the path of each piece of evidence;
- the **goal**: who it is for, and what success looks like for them;
- what is already decided, and what is out of scope;
- the questions to answer, or "judge it whole";
- the verdict shape below.

Members work read-only: they open files, run read-only commands, and browse the running app, and change nothing.

When the user asks for roles (a first-time player, an accessibility reviewer), end each member's brief with its role; everything above the role stays identical.

**Verdict shape**, asked of every member:

- **Verdict**: `ship`, `revise`, or `rethink`, with a score out of 10.
- **Issues**, most serious first: what is wrong, why it matters for the goal, and the concrete change.
- **Keep**: what works and must survive revision.
- **Assumptions** made where the brief was silent.

## 4. Convene

In T3 Code, start one `delegate_task` per member, all at once, and follow each one's `next`. Elsewhere, use the host's subagent tool for each provider.

While members run, check each verdict against the subject as it arrives: the code, the screenshots, the numbers. Edit nothing a running member is still judging. A member the user marks non-blocking is not waited for; its late verdict is feedback, unscored.

## 5. Decide

Sort every issue from every member:

- **Adopt**: real, serves the goal, within scope. When members disagree, take a side and give the reason; agreement is evidence, not a vote, and a lone member can be right.
- **Decline**, with the reason: wrong on the facts, outside scope, or taste the goal does not settle.
- **Ask the user**: a fork only they can settle, such as scope added or dropped, reversing a decision they made, or a taste call with a fair case on each side.

Guard the scope. Asked to judge a plan, a council proposes a bigger one. Adopt what makes the subject meet its goal and list the rest as later ideas.

## 6. Revise and reconvene

Apply what you adopted: revise the plan file, or change the code and retake the screenshots when the user's request covers building it. Then convene a fresh round on the revised subject, with the same brief plus a list of what changed and what you declined and why, so a member argues with your reasons instead of re-raising the point.

A **round** is one full pass by every member. Stop when every member says `ship`, after three rounds, or after one when the user asked a question rather than for work to be judged (which of two mechanics, whether to build it at all).

## Report

- The council's members, and a table with each member's verdict and score per round. A member that gave no score gets `no score`; never invent one.
- Where the final subject lives: the revised plan's path, the branch, the screenshots.
- Adopted, declined with reasons, and still contested with each side's case.
- The questions for the user.

When the work ends in a pull request, put the table in its description under `## Council review`, above the closing model and harness line, by editing the description rather than commenting. See [examples.md](examples.md).
