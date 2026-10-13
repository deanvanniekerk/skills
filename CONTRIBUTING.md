# Contributing

Open an issue with the skill, agent/CLI versions, sanitized request, expected result, and observed failure. A small reproducible example is more useful than a broad request to make a skill stricter.

For a pull request:

1. Change the agent instructions in `SKILL.md`; update the human README/example when behavior changes.
2. Keep cross-skill dependencies explicit and provide a useful missing-dependency outcome.
3. Preserve upstream attribution and license notices. Put notices needed by an individual installation inside that skill's folder.
4. Check relative Markdown links and YAML frontmatter. Confirm discovery with `npx skills@latest add . --list`.
5. Install into a disposable project with `npx skills@latest add /absolute/path/to/this/repo --skill '*' -a codex -y` and check that references, examples, and notices survive installation.
6. Exercise a realistic scenario for the changed decision. Record what you actually tested; do not imply that a local packaging check ran a paid review or published a PR.

Keep executable validation local. This repository does not need deployment infrastructure to distribute Markdown skills.
