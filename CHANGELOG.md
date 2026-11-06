# Changelog

## Unreleased

- `quality-control`, `tackle-spec`: a subagent makes one `babysit` pass and hands back with checks still running; the orchestrator watches the PR until settled, since T3 lets no delegated subagent watch or poll one. `tackle-issue` gains the delegated mode this relies on, and waits on its own long commands in the foreground. A handback that says it is waiting on its own background command is left to resume by itself rather than nudged.
- `quality-control`, `tackle-spec`: questions go to the user in a message rather than a blocking question tool while any subagent runs, so settled PRs keep merging. Every brief tells a subagent to tear down any local environment it started.
- `tackle-spec`: every ticket is audited at once before the first starts, and all product questions go to the user in one round; a ready ticket is re-checked only when a merged PR it builds on lists Deviations that touch it. One PR per ticket, split only when the user will read it by hand (then stacked). Tickets that each generate a numbered file, such as a migration, run one after the other.
- `quality-control`: a plan over about 60 cases runs as up to three lanes, one tester and one disposable environment each. Defect FAILs go to the fixer at once; the user's change decisions follow to the same fixer and PR.
- `test-plan`: observe cases only for departures a user can see in the running app; the baseline cites CI's run on the pinned commit instead of rerunning it; expected results follow the state earlier cases leave; a gesture whose timing the device tool cannot hold goes to a real-device check. A disposable environment's teardown replaces the restore checklist, which stays only for a shared or persistent environment; a large plan is split into self-contained lanes.
- `execute-test-plan`: a gesture whose timing the device tool cannot hold is BLOCKED, not a FAIL. In a disposable environment the clean-up is its teardown, with no data restore. A lane brief runs only that lane, and the Playwright fallback also covers a browser tool that fails on its first three tries.
- `tackle-spec`: the audit checks each ticket for red flags (product questions, premises the code contradicts, names later tickets rely on) instead of tightening every detail; settled flags become the ticket's binding Decisions, and the rest of the ticket is a plan the implementer may depart from, listing why under Deviations.
- `tackle-spec`: new user-invoked skill for T3 Code. Delivers a spec issue end to end: reads its sub-issues as a blocking graph, tightens each ticket against the current default branch with product questions to the user, tackles it with `tackle-issue` in a subagent (up to two at once), merges each PR once settled, then runs `quality-control` over the spec. Leaves the parent open for the user. An issue with no sub-issues runs as a spec of one ticket, for a big ticket that earns the full pass.
- `spec-review`: new user-invoked skill. Consolidates a spec's merged tickets after agents built them independently: an inventory of everything the range adds with its siblings, judo moves that collapse duplicated code, harmonisation with the codebase, then seams, coverage and drift. Ships one behaviour-preserving PR with open questions for anything that would change behaviour. Replaces `thermo-nuclear-code-quality-review` as `quality-control`'s review step.
- `quality-control`: runs at spec level once a spec's tickets merge. Briefs carry the scope's recorded decisions as binding and route open questions and observations to the user in one round before the fix PR; Decide sorts each FAIL into a fix, a plan correction or a product question. Briefs name skills by their installed absolute path, the tester runs in the checkout the plan names, merges use the repository's own merge method, and an unfinished handback resumes the same subagent.
- `execute-test-plan`: uses the browser the environment provides, with Playwright as a disclosed fallback. Adds the OBSERVED result for observe cases and the evidence rules a built-in browser needs (saved PNG files, measured widths, fresh snapshots).
- `test-plan`: adds observe cases for open questions and departures the work already records, fixes the screenshot marker as 📸, adds OBSERVED to the report template, and asks for a baseline that runs the tests rather than replaying a cache.
- `quality-control`: new user-invoked skill for T3 Code. Orchestrates a thermo-nuclear review and fix PR, a test plan, a headless browser test run, and a fix PR for failures; subagents own and babysit their PRs, and the orchestrator merges each once settled.
- `execute-test-plan`: new skill. Runs a written test plan in a headless Playwright browser with screenshot evidence, restores test data, and removes its Playwright install when done.
- `claude-code-pr-review`: focus on concrete defects, make nits opt-in, deduplicate against freshly fetched discussions before posting, and keep local receipts with measured runtime and finding dispositions.
- `tackle-issue`: pass earlier review context to Claude, record narrow mechanical skips, and keep optional nits out of release gates. Required review failures still block completion.

## 0.1.0 — 2026-10-13

Initial public collection of five skills previously maintained as personal global skills.

- Added human-facing documentation, synthetic examples, dependency installation, and license/attribution files.
- `blast-it`: reuse existing commits, resolve open PRs explicitly, and distinguish missing PRs from API failures.
- `prune-tests`: support audit-only requests and require evidence before discarding regression or boundary coverage.
- `claude-code-pr-review`: use a pinned temporary snapshot, support fork PRs, restrict Claude to read tools, and validate findings before the calling agent posts them. Default model changed from a fixed model ID to `opus`; explicit user choices still win.
- `tackle-issue`: check dependencies early, prune and commit before local review, and report human/infrastructure blockers rather than polling indefinitely.
- `test-plan`: distinguish planning from execution, keep credentials and personal data out of artifacts, and make shared publication opt-in.

The unused `trim-agent-context` skill is not part of this collection.
