# Tackle Issue

Turn one GitHub issue into a reviewed, verified PR, and work through CI and reviewer findings until the PR is ready to merge or needs human action.

## Install the complete workflow

```bash
npx skills@latest add deanvanniekerk/skills --skill tackle-issue blast-it babysit-pr -g -a codex
npx skills@latest add mattpocock/skills --skill code-review setup-matt-pocock-skills -g -a codex
```

Run `setup-matt-pocock-skills` in the target project once, so the upstream review skill has issue-tracker configuration. Use the same agent and global/project scope for every dependency. Already installed upstream plugins can satisfy these dependencies; avoid duplicate copies. Skills CLI installation of this skill does not automatically install its dependency chain.

Requires Git, authenticated GitHub CLI with push/review access, the project's dependencies, and verification commands. The review round runs in subagents, so the host must support them. The defects review runs a Claude Opus subagent and a Codex GPT Astra one side by side, so the host must be able to start both: [T3 Code](https://github.com/pingdotgg/t3code) does from either provider. Plain Claude Code or plain Codex cannot, and the flow stops at setup there. A Claude review of the published PR is opt-in and needs `claude-code-pr-review` with authenticated Claude Code. Missing required dependencies are identified before implementation.

## Use

> Use tackle-issue on issue 42. Implement the acceptance criteria, publish the PR, and address review/CI findings. Do not merge.

The issue number resolves against the current repository; use a full issue URL to disambiguate. The issue needs enough acceptance criteria to distinguish correct implementation from a plausible guess.

## The sequence

```text
issue → implement → simplify → prune → commit → one review round
      → merge default branch → full verify → publish ready PR → follow reviews and CI
```

Simplification is a behavior-preserving cleanup of the changed code, and pruning removes the new tests that detect nothing a regression would break, so reviewers see the cleaned diff. Both follow the skill's own short brief; `prune-tests` remains available for a full audit.

The review round runs once, before publication, after the changes are committed: `code-review`'s Standards and Spec subagents and two defects reviewers (state, races, invariants, cross-package and schema behavior), Claude Opus and Codex GPT Astra on the same brief, start together, each with a fresh context. The report lists each defects reviewer's findings separately (fixed, declined, found by both), so you can compare the two models. Findings are fixed before the PR opens, so the full verification and the PR's own reviewers see the reviewed code. Between steps only the affected tests and the typecheck run; the full verification runs once, on the head being published after the default branch is merged into it, because that merged result is what lands. `blast-it` then reuses those commits. The flow chooses routine details autonomously and records them; it stops for material ambiguity, missing access, failed verification, or an action beyond scope.

It creates commits, pushes, and fixes verified review issues. It never merges as part of this workflow. Briefed by an orchestrator that watches the PR (`tackle-spec`, `quality-control`), it makes one follow-up pass and hands the PR back with checks still running. Follow-up ends when the PR meets the criteria, closes/merges externally, requires human action, or remains blocked after two attempted remedies. Continuing later requires a supported monitor and authorization.

Only an entirely mechanical diff skips the review round: comments, formatting, or prose that changes no procedure. A finding is fixed when it is real and within the issue; the rest is declined with a reason and listed as a follow-up, so review does not grow the PR. Ask for a Claude review in the request to also run `claude-code-pr-review` on the published PR.

## Expected result

Issue and PR URLs, head SHA, implementation summary, checks, tests pruned, review findings fixed and declined (each defects reviewer's listed under its model), and either readiness or the exact blocked phase. A partial or failed phase is not silently skipped.

See [the example](examples.md) and [agent instructions](SKILL.md). The workflow depends on upstream skills that may change over time. [Credits](CREDITS.md), [Dean's MIT license](LICENSE.txt), and [Matt Pocock's MIT notice](LICENSE-MATT-POCOCK.txt).
