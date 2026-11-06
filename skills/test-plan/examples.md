# Example handoff excerpt

This is a fictional example, not runnable setup for a real repository. A generated plan must replace every setup fact with evidence from its target project.

## Scope

Feature: confirm an order with delivery-address validation. Pin the actual reviewed commit in the finished plan. Requirements: incomplete addresses are rejected; complete addresses can be confirmed; unrelated orders remain unchanged.

## Cast and environment

Use a disposable local fixture account with the order-manager role. Select one draft order with a complete address and another missing a required address field. Record how the target project's seed command creates them and how to look up their IDs. Configure mail/webhooks to the project's local sink before execution. The run uses a disposable local environment, so its teardown is the command that destroys it, and no case restores data.

This excerpt has no repository evidence for those setup commands, so environment setup is **BLOCKED** until supplied. It does not invent a seed command, login, or port.

## Cases

| ID | Actor and steps | Expected result | Evidence |
| --- | --- | --- | --- |
| ORD-01 | Manager attempts confirmation of incomplete-address order | Validation refusal; order remains draft | Capture refusal and persisted state |
| ORD-02 | Manager confirms complete-address order | Order becomes confirmed and remains confirmed after reload | Capture state before/after reload |
| ORD-03 | A role without confirmation permission calls the documented confirmation API | API refuses; order unchanged | Record status/body |
| ORD-04 📸 | Manager opens the orders list at 1280 px (observe: the PR's Deviations narrowed the Cancel column) | Record whether the table scrolls sideways and whether Cancel is visible without scrolling | Screenshot |

The full plan supplies the confirmed API endpoint/payload, exact refusal contract, role login procedure, fixture reset, and teardown command for the target project. It names actual automated coverage for concurrency and records a coverage gap if none exists.

## Report template

```text
Commit/build:
Lane:
Environment and baseline:
ID | PASS/FAIL/BLOCKED/OBSERVED | evidence | notes
Failures:
Observations:
Plan corrections:
Torn down / left behind:
```
