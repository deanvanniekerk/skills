# Spec Consolidate

Turn `spec-review` findings into one behaviour-preserving PR. [spec-review](../spec-review/README.md) reads a spec's merged range and writes what it finds to a file; several reviewers on different models can read the same range side by side. This skill takes all their files, merges the findings they share, checks each one against the code itself, applies the confirmed ones, and opens the PR.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill spec-consolidate --skill spec-review -a codex -a claude-code
```

It judges findings by `spec-review`'s rules, so install the two together. With [blast-it](../blast-it/README.md) installed it publishes through it.

## Use

The skill is user-invoked only; `quality-control` runs it after its reviewers.

> /spec-consolidate on spec #40 with ~/spec-reviews/40/opus.md and ~/spec-reviews/40/astra.md

## Expected result

One PR listing each consolidation, harmonisation and bug fixed, the open questions where a change would alter behaviour or a recorded decision, follow-ups too large for one PR, and the findings discarded. With nothing confirmed, no PR. The report ends with a disposition table: every finding in every file, what became of it, and which other files raised it, so you can compare the reviewers.

See [the example](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
