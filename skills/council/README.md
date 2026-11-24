# Council

Put an idea, plan, design, or piece of work before models from different providers, then weigh what they say and revise. The subject can be anything an agent can show: game mechanics, a UI in the running app, a spec, an architecture, code.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill council -g -a codex -a claude-code
```

Needs a host that can start a model from at least two providers. T3 Code with Claude and Codex enabled runs it from either. No other skills are required.

## Use

> Council this stamina mechanic before I build it.

> Get a council on the new settings page at phone and desktop width.

> Council the plan in docs/sync.md with Opus, GPT Sol and GPT Astra. One round only.

The agent seats one member per provider (by default the newest Claude Opus and Codex GPT Sol) unless you name them. It writes the subject's evidence to files (the full plan, screenshots per width and state, the numbers behind a mechanic) and gives every member the same brief, so each judges independently. Members are read-only. The agent adopts, declines with a reason, or brings to you each issue raised, revises the subject, and reconvenes until every member says ship, for at most three rounds.

It guards the scope: ideas that make the subject bigger instead of better are listed for later, not adopted.

## Expected result

A table of each member's verdict and score per round, where the revised subject lives, what was adopted, declined, and still contested, and the questions only you can settle. When the work ends in a PR, the table goes in its description.

See the [agent instructions](SKILL.md) and [examples](examples.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
