# Attribution and dependencies

Dean van Niekerk maintains the five skills in this repository. Publication preparation distinguished local custom skills from Skills CLI-managed upstream installations; the latter are deliberately not vendored into this collection.

## Adapted material

`tackle-issue` originally incorporated the implementation instructions from [Matt Pocock's implement skill](https://github.com/mattpocock/skills/tree/main/skills/engineering/implement). The public version retains the workflow's influence while revising pruning, commit order, dependency checks, review, and completion handling. Matt's copyright and MIT license are preserved in [the installed skill's license notice](skills/tackle-issue/LICENSE-MATT-POCOCK.txt).

The inspected local installation recorded the `implement` folder Git tree hash `f07d230f645fc9ac390cf13a450bbff12ad791a3`. This identifies the audited installation, not a promise of compatibility with every future upstream release.

## External dependencies

| Author/project | Skills used | Relationship |
| --- | --- | --- |
| [Matt Pocock](https://github.com/mattpocock/skills) | `pr`, `tdd`, `code-review`, `codebase-design`, `setup-matt-pocock-skills` | Installed separately; provide PR writing, test discipline, local review, terminology, and project setup |
| [thedotmack / claude-mem](https://github.com/thedotmack/claude-mem/tree/main/plugin/skills/babysit) | `babysit` | Installed separately; follow-up on PR checks and review threads |

Matt's `pr` skill in turn [credits Dex Horthy / HumanLayer's show-me material](https://github.com/mattpocock/skills/blob/main/skills/engineering/pr/CREDITS.md). That content remains in its upstream package and is not copied into this repository.

The dependency audit on 13 October 2026 used these locally installed folder hashes:

| Skill | Recorded Git tree hash |
| --- | --- |
| `pr` | `00b896d811adffad747c7ca30078d909d60c7f93` |
| `tdd` | `bf1bf5ffcd63aa8abce830f2955a853c121fb463` |
| `code-review` | `d8e341cee7980127dddda05159bedf25dc853615` |
| `codebase-design` | `07ba1cd60efc12d0d3a083005c1d9651cf15e39a` |
| `setup-matt-pocock-skills` | `b1568cde855a189b16e5d7dd8a0fa4c3f156bc2e` |
| `babysit` | `b7fe9ecec0a9ee15c6964fe9b0d1489a05ae6bdb` |

These are provenance records, not CLI version pins or end-to-end test results. Check upstream changes when updating a composed workflow.

## Related work and tools

- [OpenAI's yeet](https://github.com/openai/skills/tree/main/skills/.curated/yeet) is a related GitHub publishing workflow used in Dean's environment. `blast-it` is maintained separately; `yeet` is neither bundled nor required.
- [Vercel's Skills CLI](https://github.com/vercel-labs/skills) provides installation and discovery.
- [Anthropic's Claude Code](https://code.claude.com/docs/en/cli-reference) performs the headless review.
- [GitHub CLI](https://cli.github.com/) supplies repository, issue, and pull-request operations.

Credits acknowledge sources and dependencies, not endorsement. Third-party projects retain their own licenses. If incorporating their files in a future change, preserve the corresponding notices inside the installable skill folder as well as documenting the source here.
