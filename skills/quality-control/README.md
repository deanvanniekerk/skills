# Quality Control

Put a spec through a full quality pass with one command, once its tickets have merged. [tackle-spec](../tackle-spec/README.md) runs it as its last stage. Each ticket was already reviewed on its own (for example by `tackle-issue`); this pass checks the spec as a whole. The agent you start orchestrates; subagents do the work:

1. A Codex GPT Astra subagent runs `spec-review` over the spec's merged range and writes what it finds to a file: the code the tickets duplicated, what does not fit the codebase, and the bugs where tickets meet. A Claude Opus subagent runs `spec-consolidate` on that file: it merges the shared findings, checks each against the code, applies the confirmed ones, opens a PR, and fixes what review and CI raise; the orchestrator watches it and merges it once settled.
2. Another Opus subagent writes a manual test plan: a smoke plan of about 20 cases in one lane by default, or a thorough one when you ask for thorough testing, split into up to three lanes when it is large and the repository can run local slots side by side.
3. A GPT Sol subagent per lane runs that plan in a browser, each on its own slot, which it releases when done, with no data restore.
4. The orchestrator decides open questions and observations with a clear route itself, and puts the real forks to you in one round, in a message.
5. If the run found failures, another Opus subagent starts fixing them at once, and the changes you ask for follow it. It opens a PR and fixes what review and CI raise; the orchestrator watches it and merges it once settled.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill quality-control -a codex -a claude-code
```

Install the skills it chains into the same project:

```bash
npx skills@latest add deanvanniekerk/skills --skill spec-review --skill spec-consolidate --skill blast-it --skill babysit-pr --skill test-plan --skill execute-test-plan -a codex -a claude-code
npx skills@latest add mattpocock/skills --skill implement -a codex -a claude-code
```

Requires [T3 Code](https://github.com/pingdotgg/t3code) for its subagent, PR-watch, and pending-request tools, with Claude (Opus) and Codex (GPT Sol, GPT Astra) providers available. Also requires authenticated `gh` with push and merge rights. It stops if a named model is unavailable rather than substituting another.

## Use

The skill is user-invoked only; an agent never starts it on its own.

> /quality-control on spec #746

> /quality-control on PRs 1917, 1918 and 1920

> /quality-control on spec #746, test it thoroughly

> /quality-control on everything merged since v1.4.0

## Expected result

Up to two merged PRs, one with the review's consolidations and fixes and one with the test fixes and the changes you decided, plus a test plan, a test report, and screenshot evidence saved outside the repository. Decisions already recorded in the issues and PRs stay as they are; anything that would change one comes to you as a question first. The final report lists each PR merged, the review findings applied, what became of each of the reviewer's findings (applied, discarded, open question, follow-up), your decisions, the decisions it made for you, PASS/FAIL/BLOCKED/OBSERVED counts, fixes shipped, plan corrections, the causes of BLOCKED cases, and anything left behind.

It merges to the default branch without asking again, using the repository's usual merge method, and runs several high-effort agents, so expect a long and costly run.

See [the example](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
