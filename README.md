# Dean's engineering skills

Portable agent workflows for implementing, reviewing, publishing, and testing code changes.

| Skill | What it does |
| --- | --- |
| [test-plan](skills/test-plan/README.md) | Writes a manual QA plan |
| [prune-tests](skills/prune-tests/README.md) | Audits or removes low-value tests |
| [claude-code-pr-review](skills/claude-code-pr-review/README.md) | Runs Claude Code review and posts validated findings |
| [blast-it](skills/blast-it/README.md) | Commits, pushes, and opens or updates a PR |
| [tackle-issue](skills/tackle-issue/README.md) | Takes an issue through implementation, review, and CI; stops before merging |

## Install

Requires Node.js and npm. Choose your skills and agent interactively:

```bash
npx skills@latest add deanvanniekerk/skills
```

Use `-g` for global installation, `--skill <name>` for one skill, and `-a <agent>` to select an agent. For example:

```bash
npx skills@latest add deanvanniekerk/skills --skill test-plan -g -a codex
```

`blast-it` and `tackle-issue` need additional skills; see their linked READMEs for setup. Publishing and GitHub review require authenticated `gh`. Claude review also requires authenticated Claude Code and uses your Claude allowance or API usage.

## Use

Ask your agent:

> Use test-plan to prepare a QA handoff for PR 42.

> Use tackle-issue on issue 42.

## Update

```bash
npx skills@latest update -g
```

Maintain changes in this repo; updates can overwrite edits to installed copies.

By [Dean van Niekerk](https://github.com/deanvanniekerk) · [MIT](LICENSE) · [Credits](ATTRIBUTION.md) · [Changelog](CHANGELOG.md) · [Contributing](CONTRIBUTING.md)
