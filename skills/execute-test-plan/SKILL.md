---
name: execute-test-plan
description: Execute a written manual test plan in a browser, with screenshot evidence and a filled report. Use when asked to execute or run a test plan.
license: MIT
---

# Execute Test Plan

1. **Read** the plan fully, then start the apps it names.
2. **Run** every case in plan order by driving the app in a browser. Record PASS / FAIL / BLOCKED, and OBSERVED for an observe case, which has no pass/fail. Wrong behaviour is a FAIL; a label or placement that differs from the plan is a plan correction. A case you cannot drive is BLOCKED with the exact error, including a timed touch gesture (double-tap) that a simulator's device tool sends too slowly to register.
3. **Clean up** as below.
4. **Report** in the plan's report template, saved where the plan or your brief says, and return it as your final message.

Application code stays unchanged; nothing is committed or pushed. Prefix every record you create with `QA `.

## Browser

Use the browser tool your environment provides. With none, set one up from [playwright.md](playwright.md).

- Sign in through the UI and use the real controls; assert what is rendered.
- Evidence is a PNG file at `<evidence>/<case ID>.png`, for every 📸 case and every FAIL, taken once the expected result is on screen. Save it and copy it there; a screenshot shown only inline is not evidence.
- Screenshots may be scaled. Confirm a viewport width by measuring it in the page (`innerWidth`), not from the image or the resize reply.
- Element references go stale when the page re-renders or a popup covers the control: take a fresh snapshot and act again.
- Roles share one browser profile's storage. Sign out before signing in as the next role, or give each role its own profile.
- Phone is 390 × 844, a narrow window 800 wide; dark theme is the browser's colour-scheme setting.

## Clean up

- Work through the plan's restore checklist. Put every setting you changed back to its original, such as a role, an assignment or a note.
- Remove your `QA ` records the way a user would, such as cancelling a document in the UI. Delete one directly only after the plan's checks show nothing references it.
- Stop every server you started. Close every browser tab you opened and reset the colour scheme; with the Playwright fallback, remove it as that file says.
- Keep the report and the evidence screenshots.
- List in the report what you cleaned up and anything you had to leave behind.
