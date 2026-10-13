# Dean's engineering skills

Agent workflows I use to move a change from an issue to a reviewed pull request, and hand it to someone who can test it cold.

Written and maintained by [Dean van Niekerk](https://github.com/deanvanniekerk). These skills capture decisions I want an agent to make consistently: what earns a test its place, what makes a useful review, and what evidence belongs in a testing handoff.

## Pick a workflow

| Skill | Outcome | What it changes |
| --- | --- | --- |
| [test-plan](skills/test-plan/README.md) | A manual QA plan with roles, fixtures, expected results, and cleanup | Writes a local document; does not execute the plan |
| [prune-tests](skills/prune-tests/README.md) | A case-by-case test-value audit, or a smaller suite with coverage preserved | Audit mode reads; prune mode deletes confirmed redundant tests |
| [claude-code-pr-review](skills/claude-code-pr-review/README.md) | Claude findings grounded in a pinned PR snapshot | Posts one validated GitHub review; dry-run stays local |
| [blast-it](skills/blast-it/README.md) | A verified branch published as a ready PR | Stages, commits, pushes, and creates/updates a PR |
| [tackle-issue](skills/tackle-issue/README.md) | An issue implemented, reviewed, and followed through CI | Runs the delivery sequence through a merge-ready PR; does not merge |

Start with `test-plan` or an audit-only `prune-tests` request. `tackle-issue` composes the other workflows and needs the dependencies below.

## Install

With Node.js and npm available, use the [Skills CLI](https://github.com/vercel-labs/skills):

```bash
# Choose skills and agents interactively; defaults to project installation
npx skills@latest add deanvanniekerk/skills

# Install one skill globally for Codex
npx skills@latest add deanvanniekerk/skills --skill test-plan -g -a codex

# Inspect the catalogue without installing
npx skills@latest add deanvanniekerk/skills --list
```

Use `-a claude-code` for Claude Code, or omit `-a` to choose your agent. Omit `-g` to install into a project. Installation makes the instructions available; it does not configure GitHub, buy Claude access, or install other skills automatically.

## Install the delivery workflow and its dependencies

Use the same scope and agent for each command:

```bash
npx skills@latest add deanvanniekerk/skills --skill '*' -g -a codex
npx skills@latest add mattpocock/skills --skill pr tdd code-review codebase-design setup-matt-pocock-skills -g -a codex
npx skills@latest add thedotmack/claude-mem --skill babysit -g -a codex
```

Then run Matt's `setup-matt-pocock-skills` once in the target project to configure the issue tracker and conventions his review skill expects. Review the proposed project setup as you would any configuration change. If those upstream skills already come from a plugin, use that installation and avoid duplicate copies.

| Our skill | Other skills required | Tools/access |
| --- | --- | --- |
| `test-plan` | None | Repository/spec access; GitHub access when the scope is a PR or issue |
| `prune-tests` | None | Git for branch scope; the project's test/lint/typecheck tools |
| `claude-code-pr-review` | None | Git, authenticated `gh`, authenticated Claude Code; write access to post reviews |
| `blast-it` | Matt's `pr` | Git, authenticated `gh`, push access, project verification tools |
| `tackle-issue` | This repo's pruning/publishing/review skills; Matt's `tdd`, `code-review`, `codebase-design`, `pr`; thedotmack's `babysit` | All of the above, plus upstream repository setup |

## Use

Ask your agent directly, or select the installed skill using its skill picker:

> Use test-plan to prepare a QA handoff for PR 42. Save it locally; do not run the tests.

> Use prune-tests to audit the tests changed on this branch. Give me the verdicts without editing files.

> Use tackle-issue on issue 42. Implement it, publish the PR, and follow reviews and CI until it is ready. Do not merge.

Each skill's README has prerequisites, examples, expected output, and limitations. The `SKILL.md` beside it is the instruction set the agent executes. Examples are synthetic and are not reports of live runs.

## Updates and local development

```bash
# Update installed global skills
npx skills@latest update -g

# Rerun add to select newly introduced skills
npx skills@latest add deanvanniekerk/skills -g -a codex
```

Keep edits you want to maintain in a clone of this repository and submit changes there. Treat installed copies as replaceable: updates can replace local customizations. Read the [changelog](CHANGELOG.md) when updating a delivery workflow.

## Compatibility and validation

These are portable Markdown agent skills. They need an agent that can read repository files and, for execution, run the documented tools. Slash commands and skill selection vary by host. External skills can require additional host features such as subagents.

The public package is checked for valid skill frontmatter, complete local links, Skills CLI discovery, and isolated installation. Full delivery against a live issue, paid Claude review, and GitHub review posting are not covered by that packaging check. Follow your host's permissions and your repository's procedures; installed instructions do not override either.

Claude review defaults to the `opus` alias with high effort and consumes the user's Claude allowance or API usage. Its README documents the reviewed CLI controls and model override.

## Authorship

These five workflows are maintained by Dean. They build on ideas and installed skills from other authors. `tackle-issue` adapts Matt Pocock's `implement` workflow and includes his MIT notice. Upstream skills are installed from their authors, not repackaged here. See [ATTRIBUTION.md](ATTRIBUTION.md) and the per-skill credits for the distinctions.

MIT licensed. Each installed skill carries its own license copy. Contributions and concrete failure reports are welcome; see [CONTRIBUTING.md](CONTRIBUTING.md).
