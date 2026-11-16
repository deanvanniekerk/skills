# Test Plan

Write a manual QA handoff that another tester can execute without knowing the codebase. The plan provides concrete environment, role, fixture, API, and expected-result details.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill test-plan -g -a codex
```

No other skills are required. Give the agent repository/spec access and GitHub access when the scope is a PR or issue. Running the application is not required to author the plan; unconfirmed runtime facts must be marked as blockers.

## Use

> Use test-plan for PR 42. Write a Markdown handoff for another testing agent. Do not execute it.

> Write a thorough test plan for PR 42.

> Prepare a test plan for everything since the last release tag, including role boundaries. It runs on local slots. Save it in my chosen output directory.

The skill reads code, specifications, and fixture definitions. It does not reset a database, drive the application, send mail, or execute the resulting cases. If you also want execution, ask separately. Public/shared rendering is opt-in.

## Expected result

A Markdown plan pinned to a commit, with setup, synthetic test accounts, feature suites, and a pass/fail/blocked/observed report template. By default it is a smoke plan: about 20 cases in one lane covering open questions, each feature's main path, the seams between changes, and the refusals that guard something hard to undo, with the areas left to automated tests named. Ask for thorough or full testing to get every requirement mapped to a case, the permission matrix, automated-only coverage linked test by test, and lanes for a large plan when the repository can run local slots side by side. The run restores no data: it ends by tearing down its disposable environment, such as releasing its slot, and in a shared one the tester puts back the settings its cases changed. Open questions, and departures the work already records that a user can see, become observe cases: the tester records what happens rather than passing or failing it. It also reports unresolved facts and direct data edits the later run would require.

Disposable seed credentials may be documented when they are intentionally public test data. Real credentials are referenced through their approved retrieval mechanism; personal records and production secrets do not belong in a portable handoff. A source-derived plan is not evidence that its test cases passed.

See [the example handoff](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
