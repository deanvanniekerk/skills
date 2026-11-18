# Spec Review

Review a spec once its tickets have merged. When agents build a spec's tickets independently, each one is reviewed against its own ticket, and the merged result usually solves the same idea several ways. This review reads the whole merged range and finds the judo moves that collapse duplicated code into one, so the spec fits the rest of the codebase. It also checks where the tickets meet, what the spec asked for, and docs or tests a later ticket left wrong.

It changes no code: it writes its findings to a file, and [spec-consolidate](../spec-consolidate/README.md) applies them. That split lets several reviewers, each on its own model, read the same range side by side.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill spec-review --skill spec-consolidate -a codex -a claude-code
```

## Use

The skill is user-invoked only; `quality-control` runs it as its review step, once per reviewer model.

> /spec-review on spec #40, now that #41–#44 have merged

> /spec-review on main since v1.4.0, spec in docs/specs/orders.md

Then run `/spec-consolidate` with the findings file to get the PR.

## Expected result

A findings file outside the repository: each consolidation, harmonisation, bug, seam, coverage gap and drift, with its sites, the move, what it deletes and how it was verified, largest first; then open questions where a change would alter behaviour or a recorded decision, and the findings discarded. Decisions recorded in the spec's issues and PRs stay as they are.

See [the example](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
