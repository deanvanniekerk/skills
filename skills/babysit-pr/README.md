# Babysit PR

Stay with a pull request until it is ready to merge: checks pass, the automated reviews have finished, every review thread is answered and resolved, and the head contains the default branch.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill babysit-pr -g -a codex
```

Requires Git and authenticated GitHub CLI with push access. No other skills are required.

## Use

> Babysit PR 42.

> Use babysit-pr on this branch's PR until it is ready. Do not merge.

Each pass reads the PR, checks every finding against the code, fixes the real ones that belong to the PR, declines the rest with a written reason, pushes once, and answers and resolves each thread. Between passes it waits on the host's PR watcher (T3 Code's `watch_pull_request`) instead of polling. Briefed by an orchestrator that watches the PR, as `tackle-spec` and `quality-control` do, it makes one pass and hands back.

Review loops tend to grow a PR one small fix at a time. This skill keeps the PR at the size its purpose needs: a rare path outside that purpose, a test that pins nothing, extra compatibility, or a small cost is declined and listed as a follow-up.

## Expected result

The PR URL and head SHA, each check and its result, findings fixed and declined with reasons, any thread still open, and whether the PR is ready. It never merges, and it reports instead of waiting when only a human can move the PR forward.

See the [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
