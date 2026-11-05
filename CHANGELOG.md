# Changelog

## Unreleased

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
