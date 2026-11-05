# Spec Review

Consolidate a spec once its tickets have merged. When agents build a spec's tickets independently, each one is reviewed against its own ticket, and the merged result usually solves the same idea several ways. This review reads the whole merged range and finds the judo moves that collapse duplicated code into one, so the spec fits the rest of the codebase. It also checks where the tickets meet, what the spec asked for, and docs or tests a later ticket left wrong.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill spec-review -a codex -a claude-code
```

No other skills are required. With [blast-it](../blast-it/README.md) installed it publishes through it.

## Use

The skill is user-invoked only; `quality-control` runs it as its review step.

> /spec-review on spec #40, now that #41–#44 have merged

> /spec-review on main since v1.4.0, spec in docs/specs/orders.md

## Expected result

One behaviour-preserving PR. Its body lists each consolidation (what merged into what, and the code it deleted), the harmonisations and bugs fixed, open questions where a change would alter behaviour or a recorded decision, and the findings discarded. Decisions recorded in the spec's issues and PRs stay as they are.

See [the example](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
