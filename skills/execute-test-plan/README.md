# Execute Test Plan

Run a written manual test plan, such as one from [test-plan](../test-plan/README.md), in a browser, and fill in its report. Meant for the agent that does the testing.

## Install

```bash
npx skills@latest add deanvanniekerk/skills --skill execute-test-plan -g -a codex
```

No other skills are required. The agent uses the browser its environment provides, such as T3 Code's built-in browser. With none, it falls back to Playwright, which needs Node.js and npm, downloads Chromium (about 150 MB), and is removed again when done unless another Playwright install still uses it.

## Use

> Use execute-test-plan on the plan at `test-plans/orders-plan.md`. Save the report and screenshots beside it.

The agent starts the apps the plan names. It runs every case through real clicks and typing, with screenshots as evidence, and marks each case PASS, FAIL or BLOCKED, or OBSERVED for a case the plan only asks it to observe. Then it stops the servers and closes the browser it started, restores no data, and tears down its disposable environment, such as releasing its slot; in a shared one it puts back the settings its cases changed. Briefed with a lane of a large plan, it takes that lane's slot and runs only that lane. It does not change application code or commit anything.

## Expected result

A filled report in the plan's own template, covering every case. It includes a screenshot file for each 📸 case and each failure, plan corrections where the UI differs from the plan, the records it created or edited directly, and its teardown. A case the agent could not drive is BLOCKED with the exact error, never passed from reading code.

See [the example](examples.md) and [agent instructions](SKILL.md). Authored and maintained by Dean van Niekerk; [credits](CREDITS.md), [MIT license](LICENSE.txt).
