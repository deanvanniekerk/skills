# Changelog

## Unreleased

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
