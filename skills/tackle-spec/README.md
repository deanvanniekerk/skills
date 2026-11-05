# Tackle Spec

Deliver a whole spec with one command: a parent issue and its sub-issues, from open tickets to a merged, quality-checked whole. It also takes a single big issue with no parent, and gives that one ticket the same tightening, delivery and quality pass. The agent you start orchestrates; subagents do the work:

1. Tickets run in the order their blocking links allow, up to two at a time.
2. Before each ticket starts, a Claude Opus 5.5 subagent re-checks it against the default branch as it now stands, and turns every open choice into a decision. Product questions come to you first.
3. A second Opus subagent tackles the ticket with `tackle-issue`, then fixes what review and CI raise on its PR; the orchestrator watches it and merges it once settled.
4. Once every ticket has merged, the orchestrator runs `quality-control` over the spec: `spec-review` consolidates the code the tickets duplicated, then a test plan, a browser test run, and a fix PR.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill tackle-spec --skill tackle-issue --skill quality-control -a codex -a claude-code
```

Then install what [tackle-issue](../tackle-issue/README.md) and [quality-control](../quality-control/README.md) each chain. Requires [T3 Code](https://github.com/pingdotgg/t3code) for its subagent, PR-watch, and pending-request tools, with Claude (`claude-opus-5-5`) and Codex (`gpt-6.1-sol`) providers available, and authenticated `gh` with push and merge rights.

## Use

The skill is user-invoked only; an agent never starts it on its own.

> /tackle-spec #746

> /tackle-spec #767, three tickets at a time

> /tackle-spec #812 (one big ticket, no parent)

## Expected result

Every sub-issue closed by its own merged PR, with each ticket tightened before it started, then a spec-review PR, a test plan, a test report, and a fix PR from `quality-control`. You answer product questions in batches as they come up. The parent issue stays open for you to close.

It merges to the default branch without asking again and runs many high-effort agents, so expect a long and costly run.

See [the example](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
